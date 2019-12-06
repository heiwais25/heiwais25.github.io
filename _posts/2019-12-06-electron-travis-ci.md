---
layout: post
title: Tarvis CI를 이용한 electron test 및 자동 배포
date: 2019-12-6 21:12
categories: [electron]
author: "JongHyun"
---

`electron`을 개발한다면 배포를 위해 자연스럽게 사용하는 것은 `electron-builder`일 것이다. `electron-builder`를 이용하면 각 운영체제에 맞는 실행 파일을 손쉽게 만들어낼 수 있는데, 이 밖에도 정말 좋은 점은 Github이나 AWS S3등에 **배포**를 할 수 있다는 것이다. 이 글에서는 `electron-builder`를 사용한 Github 배포에 대해 알아보고, Travis CI를 통해 이 과정을 자동화하는 것을 알아보겠다.

## `electron-builder` publish on github

Github에 publish하기 위해서는 먼저 repository에 접근할 권한이 있는 Github token이 필요하다. 다음 [링크](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line#creating-a-token)에 자세한 설명이 적혀있는데, 개인 페이지에서 **developer settings**에 보면 access token에 대한 항목이 있다. token 설정은 **repo**만 설정해놓으면 된다. 여기서 얻은 token은 앞으로 계속 사용하게 될 것이므로 다른 곳에 적어두도록 하자. (github 계정에 대한 권한이 있기 때문에 항상 잘 관리해야 한다.)

Github token이 준비됬다면 이제 `package.json`의 `build`에 github repository에 대한 정보와 build target을 추가해주자. 이 post에서는 linux용 appImage배포만을 고려한다.

```js
{
  "build": { // build 예시
    ...,
    "linux": {
      "target": [
        {
          "target": "appImage",
          "arch": ["ia32", "x64"]
        }
      ]
    },
    "publish": [
      {
        "provider": "github",
        "owner": "{id}", // owner name
        "repo": "{repo name}" // repository name
      }
    ]
  }
}
```

이제는 다음 command를 통해 쉽게 publish할 수 있다.

```bash
GH_TOKEN={Github token} electron-builder -p always
```

위와 같이 `GH_TOKEN` 값을 설정해주면 `electron-builder`는 알아서 github에 publish하게 된다. bintray에 publish하고 싶다면 해당 token값은 `BH_TOKEN`으로 설정해주면 된다.

`electron-builder`가 github에 publish하는 과정은 다음과 같다.

1. `package.json`에 설정된 `version`값으로 build
2. 해당 `version`으로 github repository에 release draft가 있는 지 확인
3. 만약 없다면 release draft를 새로 만든다.
4. build된 결과물을 release draft의 asset으로 등록

여기서 명심해야 하는 것은 이미 같은 version의 release가 있어도 상관없지만 무조건 **release draft**이어야 한다. 만약에 draft가 아니라면 type이 맞지 않는다며 오류가 발생할 것이다. 여기까지 성공적으로 진행했다면 Github repo에 `package.json`의 version에 맞는 draft release가 생성되어있고 asset들도 함께 확인할 수 있을 것이다.

매번 이렇게 배포해도 상관없지만 개발이 끝나고 매번 이렇게 하는 건 굉장히 귀찮은 일일 것이다. 이 귀찮음을 해결하기 위해서 이번엔 **Travis CI**를 통해 push할 때마다 자동으로 publish하도록 만들어보자.

## Travis를 이용한 자동 배포

보통 Travis와 같은 CI는 github에서 push나 pull request를 할 때마다 자동으로 code test를 하기 위해 사용하는데, 우리가 사용할 기능은 code test 이후에 추가적인 deploy option이다. travis를 사용하기 때문에 추가적인 설정 파일이 필요하지만 그 안에서 돌아가는 배포 과정은 위에서 사용한 electron-builder의 publish option을 그대로 사용하게 된다.

먼저, publish과정에서 사용하게 될 `GH_TOKEN`을 Travis의 대상 branch 환경 변수에 **private**으로 저장하자. (Token이 private으로 저장되었음을 꼭 확인하자!!) 만약 Travis가 익숙하지 않다면 다음 [링크](https://hrmrzizon.github.io/2017/04/28/using-travis-ci/)를 참고하자. 이렇게 `GH_TOKEN`을 환경 변수로 설정하면 이후, Travis build과정에서는 `$GH_TOKEN`로 값을 사용할 수 있게 된다.

이제는 Token도 준비되었으므로 다음과 같이 `travis.yml` 파일을 작성하자. 가장 기본적인 예시 파일로 이 파일을 기반으로 바꾸어나가면 된다. 여기서 가정한 사항들은 다음과 같다.

1. linux 환경에서만 빌드
2. deploy는 master branch에 대해서만 수행
3. deploy를 제외한 나머지 Test는 master, release, develop branch에 대해 수행

즉, 아래 설정 중 script 항목까지는 master, release, develop branch에 대한 push, pull request가 발생하면 수행되고 master branch에 대해서만 추가적으로 deploy까지 진행된다.

```yml
# .travis.yml
language: node_js
node_js:
  - 13.1 # Node version

matrix:
  include: # 원하는 빌드 os
    - os: linux
      services: docker
      language: generic
      sudo: required
      dist: trusty

notifications:
  email: false

cache:
  yarn: true
  directories:
    - node_modules
    - $HOME/.cache/electron
    - $HOME/.cache/electron-builder

before_script:
  - git lfs pull

script:
  - |
    if [ "$TRAVIS_OS_NAME" == "linux" ]; then
      docker run --rm \
        -v ${PWD}:/project \
        -v ~/.cache/electron:/root/.cache/electron \
        -v ~/.cache/electron-builder:/root/.cache/electron-builder \
        electronuserland/builder:12-11.19 \
        /bin/bash -c "node --version && yarn install && yarn test"
    else
      yarn test
    fi

deploy:
  provider: script
  script: bash deploy.travis.sh
  skip_cleanup: true
  on: # master branch에만 deploy할 것으로 설정
    branch: master

branches: # 해당 branch에 대해서만 deploy를 제외한 나머지 사항을 공통적으로 적용
  only:
    - master
    - release
    - develop
```

위 `.travis.yml`에서 사용하는 `deploy.travis.sh` script는 다음과 같다.

```bash
# deploy.travis.sh
#! /bin/bash
if [ "$TRAVIS_OS_NAME" == osx ]; then
    # deploy on mac
    yarn build
else
    # deploy on windows and linux
    docker run --rm -e GH_TOKEN=$GH_TOKEN -v "${PWD}":/project -v ~/.cache/electron:/root/.cache/electron -v ~/.cache/electron-builder:/root/.cache/electron-builder electronuserland/builder:12-11.19 /bin/bash -c "yarn install && yarn release"
fi
```

위 script에서 주목할 점은 docker 환경을 구성할 때 `GH_TOKEN=$GH_TOKEN`으로 travis에 설정해놓은 token값을 환경변수로 설정했다는 것이다. 마지막으로 전체 프로젝트를 빌드 후 `electron-builder`까지 실행시키도록 `package.json`의 `script.release`를 설정해주자.

```js
{
  "script": {
    "build": "run-s react:build electron:build",
    "release:electron": "electron-builder --publish=always",
    "release": "cross-env NODE_ENV=production run-s build release:electron"
  }
}
```

`yarn release`를 실행하면 먼저 관련 source code를 빌드한 후에 `electron-builder`를 실행하여 publish를 진행한다. (기존의 프로젝트가 react와 electron을 사용한 것이었기 때문에 위와 같은 설정이 사용되었다.) 여기서 주목할 점은 `electron-builder --publish=always`을 통해 publish를 진행하는 것이다. 이전의 source code 빌드는 본인의 프로젝트에 맞추어 설정해놓으면 된다.

> 사실 위의 예제에서 `yarn release`로 했던 것은 `electron builder` [문서](https://www.electron.build/configuration/publish#how-to-publish)에서 `yarn release`로 실행시키면 자동으로 publish option이 추가된다는 내용때문이었는데, 왠지 option 적용이 안되어서 `--publish=always` 따로 추가하였다...

여기까지 설정을 완료했다면 master branch에 push또는 pull request를 해보자. 세부 진행 상황은 travis에서 실시간으로 확인할 수 있으며 결과물은 github에 다음과 같이 asset으로 추가된다.

<center><img src="/img/electron/191206_travis_result.png" alt="structure">
<figcaption>Fig.1 - Deploy result</figcaption></center>

---
---

이렇게 travis를 통해 자동 배포하는 것을 알아보았는데, 이 기능을 통해 electron auto update까지 할 수 있다고 한다. 관련 post는 기존에 진행중인 프로젝트에 auto update를 적용해보고 작성해야겠다 :)

## 관련 내용

1. `electron-builder` publish [링크](https://www.electron.build/configuration/publish#githuboptions)
2. `electron`과 `react`를 사용한 예시 프로젝트 [링크](https://github.com/heiwais25/Turtle)
3. travis build 결과 예시 [링크](https://travis-ci.org/heiwais25/Turtle/builds/621120467)