---
layout: post
title: create-react-app과 electron을 사용시에 경로 문제 해결
date: 2019-10-06 22:49
categories: [electron]
author: "JongHyun"
---

`electron`과 `create-react-app`를 개발 과정에서는 별 문제가 없다가도 빌드후에 `electron`에서 빌드된 react static file을 참조하는 경우에 다음과 같은 문제가 생길 수 있다.

```bash
Failed to load resource: net::ERR_FILE_NOT_FOUND
```

이는 `create-react-app` 빌드시에 기본 경로가 `package.json`의 homepage를 기반으로 설정되기 때문에 생기는 문제이다.

이를 해결하기 위해서는 간단하게 `package.json`의 homepage를 다음과 같이 설정해주자.

```json
{
    "homepage":"./"
}
```

보다 자세한 내용은 다음 [링크](https://github.com/electron/electron/issues/1769#issuecomment-272069773)에서 확인할 수 있다.
