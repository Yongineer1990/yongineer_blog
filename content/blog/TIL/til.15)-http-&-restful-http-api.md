---
title: TIL.15) HTTP & RESTful HTTP API
date: 2020-07-27 18:35:23
category: HTTP
thumbnail: './images/web.jpg'
draft: false
---

![](./images/web.jpg)

# HTTP 구조 및 핵심 요소

## HTTP

- Hyper Text Transfer Protocol
- 하이퍼텍스트 문서 (HTML)를 교환하기 위해 만들어진 Protocol (통신 규약)
    - 즉, 웹상에서 네트워크로 서버끼리 통신을 할 때 어떠한 형식으로 서로 통신을 하자고 규정해 놓은 "통신 형식" 혹은 "통신 구조" 라고 보면 된다.
    - 프론트엔드 서버와 클라이언트간에 통신에 사용된다.
    - 또한 백엔드와 프론트엔드 서버간 통신에도 사용된다.
- HTTP는 TCP/IP 기반으로 되어 있다.

## HTTP 핵심 요소

### HTTP 통신 방식

- HTTP 기본적으로 요청/응답 (request/response) 구조로 되어 있다.
    - 클라이언트가 HTTP request를 서버에 보내면 서버는 HTTP response를 보내는 구조
    - 클라이언트와 서버의 모든 통신이 요청과 응답으로 이루어 진다.
- HTTP는 Stateless이다.
    - 상태(State)를 저장하지 않는다는 뜻
    - 즉, 요청이 오면 그에 응답을 할 뿐, 여러 요청/응답 끼리 연결되어 있지 않다는 뜻이다. 즉, 각각의 요청/응답은 독립적인 요청/응답이다.
    - 그래서 만일 여러 요청과 응답의 진행과정이나 데이터가 필요할때는 쿠키나 세션 등등을 사용하게 된다.

### HTTP Request 구조

- HTTP request 메시지는 크게 3부분으로 구성된다.
    1. Start Line
    2. headers
    3. body

**Start LIne**

- HTTP request의 첫 라인
- Start Line 또한 3부분으로 구성되어 있다.
    1. HTTP Method
        - 해당 request가 의도한 action을 정의하는 부분.
        - HTTP Method에는 `GET`, `POST`, `PUT`, `DELETE`, `OPTIONS` 등등이 있다.
        - 주로 `GET`과 `POST`가 쓰인다.
    2. Request target
        - 해당 request가 전송되는 목표 url
        - 예를 들어 `/login`
    3. HTTP Version
        - HTTP 버전
        - 버전에는 1.0, 1.1, 2.0등이 있다.
        - `GET /search HTTP/1.1`

**Headers**

- request에 대한 추가 정보(addtional information)를 담고 있는 부분.
    - 예를 들어, request 메시지 body의 총 길이 (Content-Length) 등.
- key-Value 값으로 되어있다. (`:` 이 사용됨)
    - `key:value`
    - `HOST: [google.com](http://google.com)` ⇒ Key = `HOST`, Value = `google.com`
- Headers도 크게 3부분으로 이루어져 있다. (`general headers`, `request headers`, `entity headers`)
- 자주 사용되는 header
    - Host
        - 요청이 전송되는 target의 host url
        - 예를 들어, google.com
    - User-Agent
        - 요청을 보내는 클라이언트에 대한 정보
        - 예를 들어, 웹 브라우저에 대한 정보
    - Accept
        - 해당 요청이 받을 수 있는 응답(response) 타입.
    - Connection
        - 해당 요청이 끝난 후에 클라이언트와 서버가 계속해서 네트워크 커넥션을 유지 할 것인지 아니면 끊을 것인지에 대해 지시하는 부분.
    - Content-Type
        - 해당 요청이 보내는 메시지 body의 타입
        - 예를 들어, JSON을 보내면 `application/json`
    - Content-Length
        - 메시지 body의 길이
    ```wasm
    Accept: */*
    Accept-Encoding: gzip, deflate
    Connection: keep-alive
    Content-Type: application/json
    Content-Length: 257
    Host: google.com
    User-Agent: HTTPie/0.9.3
    ```

**Body**

- 해당 request의 실제 메시지 내용
- Body가 없는 request도 많다.
    - 예를 들어, GET request들은 대부분 body가 없는 경우가 많음
        ```wasm
        POST /payment-sync HTTP/1.1

        Accept: application/json
        Accept-Encoding: gzip, deflate
        Connection: keep-alive
        Content-Length: 83
        Content-Type: application/json
        Host: intropython.com
        User-Agent: HTTPie/0.9.3

        {
            "imp_uid": "imp_1234567890",
            "merchant_uid": "order_id_8237352",
            "status": "paid"
        }
        ```

### HTTP Response 구조

- Response도 request와 마찬가지로 크게 3부분으로 구성되어 있다.
    - Status Line
    - Headers
    - Body

**Status Line**

- Response의 상태를 간략하게 나타내주는 부분
- 3부분으로 구성되어 있다.
    - HTTP 버전
    - Status code : 응답 상태를 나타내는 코드, 숫자로 이루어져 있다.
        - 예를 들어, `200`
    - Status text : 응답 상태를 간략하게 설명해주는 부분.
        - 예를 들어, "Not Found"
        - `HTTP/1.1 404 Not Found`

**Headers**

- Request의 headers와 동일하다.
- 다만, response에서만 사용되는 header 값들이 있다.
    - 예를 들어, `User-Agent`대신 `Server` 헤더가 사용된다.

**Body**

- Request의 Body와 일반적으로 동일하다.
- Request와 마찬가지로 모든 response가 body가 있지는 않다. 데이터를 전송할 필요가 없을경우 body가 비어있게 된다.

```html
HTTP/1.1 404 Not Found

Connection: close
Content-Length: 1573
Content-Type: text/html; charset=UTF-8
Date: Mon, 20 Aug 2018 07:59:05 GMT

<!DOCTYPE html>
<html lang=en>
  <meta charset=utf-8>
  <meta name=viewport content="initial-scale=1, minimum-scale=1, width=device-width">
  <title>Error 404 (Not Found)!!1</title>
  <style>
    *{margin:0;padding:0}html,code{font:15px/22px arial,sans-serif}html{background:#fff;color:#222;padding:15px}body{margin:7% auto 0;max-width:390px;min-height:180px;padding:30px 0 15px}* > body{background:url(//www.google.com/images/errors/robot.png) 100% 5px no-repeat;padding-right:205px}p{margin:11px 0 22px;overflow:hidden}ins{color:#777;text-decoration:none}a img{border:0}@media screen and (max-width:772px){body{background:none;margin-top:0;max-width:none;padding-right:0}}#logo{background:url(//www.google.com/images/branding/googlelogo/1x/googlelogo_color_150x54dp.png) no-repeat;margin-left:-5px}@media only screen and (min-resolution:192dpi){#logo{background:url(//www.google.com/images/branding/googlelogo/2x/googlelogo_color_150x54dp.png) no-repeat 0% 0%/100% 100%;-moz-border-image:url(//www.google.com/images/branding/googlelogo/2x/googlelogo_color_150x54dp.png) 0}}@media only screen and (-webkit-min-device-pixel-ratio:2){#logo{background:url(//www.google.com/images/branding/googlelogo/2x/googlelogo_color_150x54dp.png) no-repeat;-webkit-background-size:100% 100%}}#logo{display:inline-block;height:54px;width:150px}
  </style>
  <a href=//www.google.com/><span id=logo aria-label=Google></span></a>
  <p><b>404.</b> <ins>That’s an error.</ins>
  <p>The requested URL <code>/payment-sync</code> was not found on this server.  <ins>That’s all we know.</ins>
```

# HTTP Method

## GET

- 이름 그대로 어떠한 데이터를 서버로 부터 받아(GET) 올 때 주로 사용하는 Method
- 데이터 생성/수정/삭제 없이 **받아오기만** 할 때 사용
- 가장 간단하고 많이 사용되는 HTTP Method
- 언급한대로 주로 데이터를 받아 올 때 사용되기 때문에 request에 body를 안 보내는 경우가 많다.

## POST

- 데이터를 **생성/수정/삭제** 할때 주로 사용되는 Method
- 데이터를 생성 및 수정 할 때 많이 사용하기 때문에 대부분의 경우 request body가 포함되서 보내진다.

## OPTIONS

- 주로 요청 URL에서 **사용할 수 있는 Method를 받아올 때** 사용된다.
- 예를 들어, `/update` url에서 어떤 메소드를 요청 가능한지 알고 싶으면 먼저 `OPTIONS`요청을 사용해서 확인하게 된다.
    ```wasm
    http -v OPTIONS http://example.org
	
    OPTIONS / HTTP/1.1
    Accept: */*
    Accept-Encoding: gzip, deflate
    Connection: keep-alive
    Content-Length: 0
    Host: example.org
    User-Agent: HTTPie/0.9.3
	
    HTTP/1.1 200 OK
    Allow: OPTIONS, GET, HEAD, POST
    Cache-Control: max-age=604800
    Content-Length: 0
    Date: Mon, 20 Aug 2018 08:37:45 GMT
    Expires: Mon, 27 Aug 2018 08:37:45 GMT
    Server: EOS (vny006/0450)
    ```

## PUT

- `POST`와 비슷하다. 데이터를 **생성 할 때 사용**되는 Method
- `POST`와 겹치기 때문에 `PUT`을 사용하는 곳도 있고 `POST`로 통일해서 사용하는 곳도 있는데, 최근 몇년 사이에 `**POST`에 밀려서 잘 사용 안되는 추세.**

# HTTP Status Code

## 200 OK

- 가장 자주 보게 되는 status code
- 문제 없이 다 잘 실행되었을때 보내는 코드

## 301 Moved Permanently

- 해당 URI가 다른 주소로 바뀌었을때 보내는 코드
```wasm
HTTP/1.1 301 Moved Permanently
Location: http://www.example.org/index.asp
```

## 400 Bad Request

- 해당 요청이 잘못된 요청일때 보내는 코드
- 주로 요청에 포함된 input값들이 잘못된 값들로 보내졌을때 사용되는 코드
    - 예를 들어, 전화번호를 보내야 되는데 text가 보내졋을때 등등.

## 401 Unauthorized

- 유저가 해당 요청을 진행 할려면 먼저 로그인을 하거나 회원 가입을 하거나 등 인증이 필요하다는것을 나타내려할때 쓰이는 코드.

## 403 Forbidden

- 유저가 해당 요청에 대한 권한이 없다는 뜻
- 예를 들어, 오직 과금을 한 유저만 볼 수 있는 데이터를 요청했을 때

## 404 Not Found

- 요청된 URI가 존재 하지 않는다는 뜻

```wasm
http -v google.com/no-such-uri

GET /no-such-uri HTTP/1.1
Accept: */*
Accept-Encoding: gzip, deflate
Connection: keep-alive
Host: google.com
User-Agent: HTTPie/0.9.3

HTTP/1.1 404 Not Found
Content-Length: 1572
Content-Type: text/html; charset=UTF-8
Date: Mon, 20 Aug 2018 08:46:48 GMT
Referrer-Policy: no-referrer
```

## 500 Internal Server Error

- 서버에서 에러가 났을때 사용되는 코드
- API 개발을 하는 백엔드 개발자들이 싫어하는 코드 🤬

# RESTful HTTP API 구조 및 핵심 개념

## 기본 배경 지식

### URI

- Unifrom Resource Identifier
- 해당 사이트의 특정 자원의 위치를 나타내는 유일한 주소.
- `/login`, `/news`
- `https://finance.naver.com/marketindex/`

### HTTP Method

- HTTP request가 의도하는 action을 정의한것.
- `POST`, `GET`

### Payload

- HTTP request에서 보내는 데이터 (body)

## REpresentational State Transfer

- 웹상에서 사용되는 여러 리소스를 HTTP URI로 표현하고 그 리소스에 대한 행위를 HTTP Method로 정의하는 방식.
    - 즉, 리소스(HTTP URI로 정의된)를 어떻게 한다 (HTTP Method + Payload)를 구조적으로 깔끔하게 표현하는것.
    - Method는 주로 `GET`과 `POST`만 사용한다.
    - `PUT`과 `DELETE`등도 사용하는 곳도 있지만, 그냥 `GET`과 `POST`만 사용하는 것이 단순하기 때문에 `GET`과 `POST`만 사용하는 추세.
- 예를 들어, 삼성전자 주식 정보를 받기 위한 HTTP 요청:
    - `HTTP GET [https://api.trueshort.com/stock/005930](https://api.trueshort.com/stock/005930)`
- 유저의 보유 주식 종목들을 DB에 저장하는 HTTP 요청:

    ```wasm
    HTTP POST https://api.trueshort.com/user/portfolio
	
    {
        "user_id" : 1,
        "stocks": [ 
            "005930",
            "298730",
            "378900"
        ]
    }
    ```

## RESTful API의 장점

- `self-descriptiveness`
- RESTful API는 그 자체 만으로도 API의 목적이 쉽게 이해가 된다.
    - 예를 들어, 위의 `HTTP GET [https://api.trueshort.com/stock/005930](https://api.trueshort.com/stock/005930)` 요청의 경우, 문서나 주석이 없이도 "[https://api.trueshort.com](https://api.trueshort.com/) 라는 API에서 삼성전자 주식에 관한 정보를 HTTP 요청을 통해 받아오는 구나" 라는 해석이 쉽게 가능하다.

## RESTful API를 개발할 때 유의 할 점

- `/`(슬래시)는 계층 관계를 나타낼때 사용된다.
    - 예를 들어, [`https://api.trueshort.com/stock/005930`](https://api.trueshort.com/stock/005930)이라는 구조라면, KOSPI에 속해있는 주식(Stock)중 삼성전자(005930)dmf skxksosms rjtdlek.
    - [`https://api.shopping.com/books/novel/stephenking`](https://api.shopping.com/books/novel/stephenking) 이라는 구조 이라면, 책들 중 소설 그리고 소설 중 Stephen King의 소설을 나타내는 구조이다.
- URI에 `_`(Underscore)는 주로 포함하지 않고 또한 영어 대문자보다 소문자를 쓴다. 그리고 너무 긴 단어는 잘 사용하지 않는다. 이 모든건 가독성을 높이기 위해서다.
- URI는 명사를 사용한다.
    - `/books/novel/stephenking` 이라고 하지 `/books/novel/get-stephenking` 이라고 잘 하지 않는다.
    - 그 이유는 동사는 `GET`, `POST` 같은 HTTP Method를 통해 표현하기 때문이다.
