---
title: TIL.43) [NodeJS] Babel 사용해보기
date: 2020-09-04 04:09:96
category: NodeJS
thumbnail: 'https://yongineer.duckdns.org/node/babel.png'
draft: false
---

![](https://yongineer.duckdns.org/node/babel.png)

# 이것은 무엇일까?

실행 환경에 구애받지 않고 항상 최신의 JS로 코딩할 수 있도록 도와 주는 도구이다.

# 왜 쓰는걸까?

JS언어의 문법과 트랜드는 항쌍 빠르게 변화하지만 정작 JS 코드를 실행하는 환경이 이를 받쳐주지 못하는 경우가 많다. 브라우저의 경우 종류 및 버전이 워낙 다양해서 어떤 문법까지 지원하는지 알기 힘들고 `NodeJS`의 경우에도 버전별로 지원하는 언어 문법이 다르기 때문에 브라우저와 비슷한 문제가 발생할 가능성이 존재한다.

따라서 개발자는 항상 최신의 문법을 사용한다면 호환성 이슈가 생길 것이고 그렇다고 호환성 이슈를 생각하여 이전 세대의 문법을 사용한다면 원치 않게 최신의 문법을 사용하지 못하게 된다.

# 그래서 Babel을 쓰는 것이다.

Babel은 JS transpiler이다. 트랜스파일이란 다른 실행 환경에서도 돌아갈 수 있도록 같은 언어를 유지한체 소스코드의 형태만 바꿔주는 것을 의미한다. Babel을 사용한다면 `ES6`이상의 최신 문법으로 작성한 JS코드를 ES5이하의 예전 문법으로 작성한 것 처럼 트랜스파일을 해줘서 개발자가 호환성 이슈를 신경쓰지 않고 최신의 문법을 사용할 수 있게 도와준다.

## Try it out

![https://yongineer.duckdns.org/js/til43_01.png](https://yongineer.duckdns.org/node/til43_01.png)

`Babel`의 홈페이지에서 제공하는 Try it out에서 `ES6`의 문법을 `ES5`이하의 문법으로 트랜스파일해보자. 이처럼 `Babel`은 최신의 문법을 자동으로 기존의 문법으로 변환해주는 기능을 한다.

# Babel 설치하기

## Node용 Bable 설치하기

프로젝트의 경로에서 다음과 같은 명령어로 설치하자.

```bash
npm install @babel/node @babel/core
```

## Preset 설정하기

그 다음은 프리셋을 설치해줘야 한다. 여기서는 env 프리셋을 사용하겠다.

```bash
npm install @babel/preset-env
```

> ⚠️Babel Presets
>
> - env : 대상 환경에 필요한 구문 변환을 세부적으로 관리 할 필요없이 최신 JS를 사용할 수 있게 도와준다.
> - flow : `Flow`를 사용하는 경우
> - react : `react`를 사용하는 경우
> - typescript : `typescript`를 사용하는 경우

## .babelrc 설정하기

`.babelrc`는 프로젝트에서 사용할 Babel Preset을 설정하는 파일이다.

먼저 프로젝트 root경로에 `.babelrc`파일을 만들어 준다. 그리고 다음과 같이 프리셋을 추가한다.

```json
// .babelrc

{
  "presets": ["@babel/preset-env"]
}
```

그리고 `package.json`의 `start`를 다음과 같이 수정한다.

```json
// package.json

...
"scripts": {
    "start": "babel-node index.js"
  },
...
```

# Babel 확인해보기

제대로 Babel이 동작하는 지 확인하기 위해 기존의 코드를 모두 ES6문법으로 바꿔보자.

```jsx
// index.js

import express from 'express'

const app = express()
const PORT = 8000

const handleListening = () =>
  console.log(`Listening on : http://localhost:${PORT}`)
const handleHome = (req, res) => res.send('hello, world!')
const hendleProfile = (req, res) => res.send('profile')

app.listen(PORT, handleListening)

app.get('/', handleHome)
app.get('/profile', hendleProfile)
```

그리고 마찬가지로 `npm start`명령으로 서버를 실행시켜보면 정상적으로 실행됨을 확인 할 수 있다.
