---
layout: post
title: electron에서 nedb를 사용시, nedb 경로 설정 방법과 build시 생길 수 있는 문제 해결
date: 2019-10-06 22:49
categories: [electron]
author: "JongHyun"
---

## neDB 경로 설정

`electron`에서 nedb와 같은 embedding DB를 사용하기 위해서는 `electron`을 배포 시에 접근 가능한 static file을 가지고 있어야 한다. 개발 과정에서는 보통 다음과 같이 경로를 설정해준다.

```js
const db = new Datastore({
    filename: isDev ? "." : `${app.getPath("userData")}/data`,
    timestampData: true,
    autoload: true
})
```

위와 같은 방식으로 neDB를 다루면 개발 과정에서는 현재 directory에, 빌드시에는 electron에 고유하게 지정되어 있는 path중 `userData` path를 사용하게 된다. 어떤 예제들에서는 기본 경로로 `app.getAppPath()`를 사용하는 경우도 있는데, 그렇게 되면 다음과 같은 문제를 낳게 된다.

## `app.getAppPath()`의 문제점

DB 경로로 위와 같은 `app.getPath()`가 아니라  `app.getAppPath()`를 사용하면 **"아마 현재 db가 없거나 cannot write"** 에러가 발생한다.

왜 문제가 발생하는지는 electron에서 배포 파일을 어떻게 관리하는지 먼저 파악해야 한다.

`electron`을 배포하기 위해서 `elctron-builder`을 사용한다고 해보자. 그렇게 electron packaging을 하면 electron에서는 기본적으로 성능 향상을 위해 `asar`이라는 format으로 파일을 압축시킨다. 사실상 진짜 파일을 압축했다고 하기보다는 electron application을 실행시에 resource를 빠르게 읽고 메모리에 올릴 수 있게 해준다. 앞에서 언급한 `app.getAppPath()`가 바로 이 `asar` file의 경로를 가리킨다.

이 경로를 그대로 DB에도 쓸 수 있었으면 좋았겠지만 문제가 하나 있는데 바로 `asar`이 바로 **Read-Only**라는 것이다. 즉, `app.getAppPath()`를 DB 경로로 사용하게 되면 file을 읽을 수는 있어도 쓸 수 없기 때문에 오류가 발생하게 되는 것이다.

요약하자면 다음과 같다.

1. Static file의 경우 `app.getAppPath()`를 쓰자.
2. DB처럼 읽고 써야하는 경우에는 `app.getPath('userData')`처럼 미리 지정된 수정가능한 경로를 사용하자.

보다 자세한 내용은 다음 링크들에서 확인할 수 있다.

> 1. `electron`에서 제공하는 경로 설정값 [링크](https://electronjs.org/docs/api/app#appgetpathname)
> 2. `electron`의 application package process [링크](https://electronjs.org/docs/tutorial/application-packaging#%EC%9D%91%EC%9A%A9-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8-%ED%8C%A8%ED%82%A4%EC%A7%95)
> 3. local db를 접근할 수 없는 이유 [링크](https://stackoverflow.com/questions/42900015/electron-packager-can-not-locate-local-db-file-after-packing/42929720#42929720)

