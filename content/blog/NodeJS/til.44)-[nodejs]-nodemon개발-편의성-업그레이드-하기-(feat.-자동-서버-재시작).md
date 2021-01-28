---
title: TIL.44) [NodeJS] nodemon개발 편의성 업그레이드 하기 (feat. 자동 서버 재시작)
date: 2020-09-04 20:09:41
category: NodeJS
thumbnail: 'https://yongineer.duckdns.org/node/nodejs.png'
draft: false
---

![](https://yongineer.duckdns.org/node/nodejs.png)

`Django`로 개발할 때는 코드 수정 시 서버가 자동으로 재시작 되어서 이게 불편할거라고는 생각도 못했는데 놀랍게도 `NodeJS`에서는 코드 수정과는 별개로 서버가 재시작 되지 않아 찾아보니 `nodemon`이라는 모듈이 있어야 한다.

새삼 `Django`가 그리워 지는 밤이다...

그럼 이 `nodemon` 간단하게 설치 및 적용하는 법을 알아보겠다.

# 설치

이 모듈은 개발용이므로 Dependencies는 당연히 `devDependencies`다 따라서 설치 시 관련 옵션을 주어 설치하자.

```bash
npm install nodemon -D
```

또는

```bash
npm install --save-dev nodemon
```

## 설치확인

`package.json`파일을 열어 `devDependencies`에 추가되었는지 확인하자.

```json
// package.json

...
"devDependencies": {
    "nodemon": "^2.0.4"
  },
...
```

# nodemon 적용시키기

이번엔 `package.json`의 `start`에서 Babel의 옵션을 추가한것 처럼 `nodemon --exec`옵션을 추가하자

```json
// package.json

...
"scripts": {
    "start": "nodemon --exec babel-node index.js"
  },
...
```

이제 서버를 시작 하고 코드를 저장하게 되면 다음과 같이 서버 재시작 메시지가 보일것이다.

![https://yongineer.duckdns.org/node/til44_01.png](https://yongineer.duckdns.org/node/til44_01.png)

> ⚠️만약 `Babel`을 사용한다면
>
>    ```json
>    // package.json
>
>    ...
>    "scripts": {
>        "start": "nodemon --exec babel-node index.js --dalay 2"
>      },
>    ...
>    ```
>
> `delay`옵션을 부여해서 `babel`이 코드를 변환 한 후 재시작 하는 방법을 추천한다.
