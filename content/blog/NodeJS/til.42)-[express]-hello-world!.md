---
title: TIL.42) [Express] Hello world!
date: 2020-09-04 01:09:48
category: NodeJS
thumbnail: 'https://yongnas.iptime.org/node/nodejs.png'
draft: false
---

![](https://yongnas.iptime.org/node/nodejs.png)

# NPM으로 Express 설치하기

먼저 프로젝트를 생성할 경로에서 npm을 시작해 준다.

```bash
npm init
```

npm이 시작되면 프로젝트에 관해 몇가지 질문을 하는데 일부 질문의 경우 기본값이 설정되어 있으며 모든 질문이 기입하지 않아도 npm이 시작된다.

npm이 시작되면 `package.json`파일이 생성 되는 것을 확인 할 수 있다. 이제 `Express`모듈을 설치하자

단, **주의할 것은 `npm`을 실행할 때 는 항상 `package.json`파일이 생성된 위치에서 실행해야 한다.** 왜냐하면 `npm`을 `package.json`파일이 없는 곳에서 실행한다면 `npm`이 `package.json`파일을 읽지 못해서 엉뚱한 곳에 `package.json`을 또 생성하기 때문이다.

그럼 다음과 같은 명령어를 실행한다.

```bash
npm install express
```

명령어를 실행하면 설치가 진행되며 잠시 후 설치가 완료 되는데 설치 후에 `node_modules`라는 폴더가 생성된 것을 확인 할 수 있다.

```bash
.
├── index.js
├── node_modules
├── package-lock.json
└── package.json
```

그리고 앞서 `npm`이 시작 되었을 때 생성된 `package.json`을 살펴보면 다음과 같은 내용이 생성되어 있음이 확인 가능하다. 이로써 `Express`의 설치는 완료되었다.

```json
// package.json
{
...
"dependencies": {
    "express": "^4.17.1"
  }
}
```

> ⚠️ Dependencies
>
> 패키지의 배포 시 포함될 의존성 모듈
>
> **devDependencies**
>
> - 패키지의 개발 시 사용될 의존성 모듈을 지정하며 배포시에는 포함되지 않는다.
>
> **peerDependencies**
>
> - 패키지의 호환성 모듈을 지정한다.
> - npm@3.x 이후에 배포 시 포함되지 않는다. 대신 호환성 모듈이 없으면 경고 메시지가 표시된다.

## .gitignore처리

`Express`설치로 인해 생성된 `node_module`에는 수많은 파일로 이루어져 있는데 `git`이 모두를 추적하기 때문에 ignore처리해줄 필요가 있다. 배포시에는 `npm install`명령 실행시 `package.json`의 `dependencies`에 명시된 모듈을 `npm`이 알아서 설치하기 때문에 무거운`node_module`을 `git`이 추적하지 않아도 된다.

또한 `package-lock.json`은 패키지에 대한 보안이 포함되어 있어 ignore처리해줘야 한다.

# Hello, world!

지금부터 `Express`를 이용해서 'hello, world!'를 출력하는 서버를 생성해보자.

```jsx
// index.js

const express = require('express')
const app = express()

const PORT = 8000

app.get('/', (req, res) => res.send('Hello, world!'))
app.listen(PORT, () => console.log(`Listening on : http://localhost:${PORT}`))
```

먼저 `express`변수에 `require('express')`를 통해 `express`모듈을 가져온 뒤 `app`변수에 이 `express`모듈을 호출함으로써 `application`객체를 생성할 수 있다.

이제 `application`객체의 `method`중 하나인 `.get()`을 통해서 `path`와 `callback`함수를 인자로 받아서 처리 해주며 이 `.get()`함수는 Route역할을 한다.

`application`객체의 또 다른 `method`중 하나인 `.listen()`은 `PORT`상수를 받아서 해당 포트의 웹 서버를 열어주게 된다. 여기서는 코드의 가독성을 위해 임의로 상수 `PORT`에 포트를 지정했지만 그냥 숫자형 인자를 전달하여도 Listening하게 된다.

## 서버 실행 해서 확인해보기

```bash
node index.js
```

프로젝트 경로에서 위와 같은 명령어로 서버를 실행 시킬 수 있지만 여기서는 `package.json`을 통해서 실행하는 방법을 포스팅 하겠다.

먼저 `package.json`을 열어 `scripts`항목에 다음과 같이 추가한다.

```json
// package.json

...
"scripts": {
    "start": "node index.js"
  },
...
```

이제 프로젝트 경로에서 다음과 같이 명령어를 입력하면 웹 서버가 실행되는 것을 확인 할 수 있다.

```bash
npm start
```

![https://yongnas.iptime.org/node/til42_01.png](https://yongnas.iptime.org/node/til42_01.png)

이제 웹 브라우저를 열어 아까 지정한 포트로 접근하여 확인해보자.

![https://yongnas.iptime.org/node/til42_02.png](https://yongnas.iptime.org/node/til42_02.png)
