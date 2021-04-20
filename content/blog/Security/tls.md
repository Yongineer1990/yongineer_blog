---
title: TLS
date: 2021-04-21 03:04:64
category: Security
thumbnail: 'https://www.cisco.com/c/dam/assets/swa/img/anchor-info/experts-secure-network-628x353.jpg'
draft: false
---

![](https://www.cisco.com/c/dam/assets/swa/img/anchor-info/experts-secure-network-628x353.jpg)
# TLS (SSL)

- `TLS`는 `AES`와 `RSA`의 단점을 보완하기 위해 등장했다.
- 처음에 대칭키를 서로 공유하는 통신을 `RSA` 비대칭키 방식을 이용하고 실제 통신을 할때는 리소스를 적게 소모하는 `AES` 대칭키 방식을 이용해 데이터를 주고 받는다.
- 또한, `HTTPS`프로토콜을 사용한다면 기본적으로 적용되어야 하는 프로토콜이다.

## TLS의 동작방식

![https://yongineer.duckdns.org/security/01.png](https://yongineer.duckdns.org/security/01.png)

`TLS`를 적용한 서버-클라이언트의 통신 패킷을 직접 확인해보면서 `TLS`의 동작방식에 대해 알아보자.

### 1. Client Hello

- Client가 Server에 접속하는 과정
- Client는 `Client Hello`라는 메시지를 전송한다.

    **Cipher Suite List**

    - Client는 Client side에서 지원하는 암호화 방식들에 대해 `Cipher Suite List`를 통해 Server side에 알려준다.

        ![https://yongineer.duckns.org/security/02.png](https://yongineer.duckdns.org/security/02.png)

    - 이 `Cipher Suite List`중 Server side와 후술할 `Server Hello` 를 통해 어떤 암호화 방식을 사용할지 결정하게 된다.

    **Random Data**

    - Client에서는 대칭키를 생성할때 사용할 난수를 생성한다.

        ![https://yongineer.duckdns.org/security/03.png](https://yongineer.duckdns.org/security/03.png)

    **Session ID**

    - `TLS`에서 `Handshake`를 하는 과정은 시간이 소요되는 작업이다.
    - 따라서 이 과정을 연결 할 때마다 수행한다면 비효율적이므로 최초 한번의 `Handshake`만 `Full Handshake`를 수행하다.
    - 이때 최초의 `Handshake`에서는 `Session ID`가 `0`이므로 후술한 `Server Hello`는 수행되지 않는다. 만약 최초의 `Handshake`가 아니라면 클라이언트는 `Server Hello`에서 전달받은 `Session ID`를 저장하게되고 이 `Session ID`를 Server에 전달하게 되어 서버에서 인가된다면 동일한 `Session ID`를 전송하게 되고 `Cipher Suite List`에서 암호화 방법을 선택하고 대칭키를 교환하는 작업을 건너뛰게 된다.

        ![https://yongineer.duckdns.org/security/04.png](https://yongineer.duckdns.org/security/04.png)

    **SNI**

    - 서버의 이름을 검증하는 단계이다.
    - 최근에는 하나의 IP로 여러개의 도메인이 연결되기 때문에 해당 IP에 접속할때 어떤 도메인을 사용하는지 확인하는 과정이 `SNI`이다.

        ![https://yongineer.duckdns.org/security/05.png](https://yongineer.duckdns.org/security/05.png)

### 2. Server Hello

- `Server Hello`는 기본적으로 `Client Hello`와 동일하다 하나 다른점이 있다면 `Server Hello`과정에서 암호화 방식을 선택한다는 점이다.

    ![https://yongineer.duckdns.org/security/06.png](https://yongineer.duckdns.org/security/06.png)

### 3. Server Certificate, Server key exchange, Server hello done

- Server의 인증서를 Client에게 보내는 단계이다. 이때 필요하다면 `CA`의 `Certificate`도 함께 전송한다.

    ![https://yongineer.duckdns.org/security/07.png](https://yongineer.duckdns.org/security/07.png)

- Server는 Client가 생성한 `Pre master secret`를 서버의 공개키로 암호화 한다.

    ![https://yongineer.duckdns.org/security/08.png](https://yongineer.duckdns.org/security/08.png)

    이때 앞서 언급한 `diffie-hellman`을 이용해 키를 교환하는 것을 확인할 수 있다.

- 모든 데이터를 전송한 Server는 `Hello Done`을 Client에게 전송하게 된다.

    ![https://yongineer.duckdns.org/security/09.png](https://yongineer.duckdns.org/security/09.png)

### 4. Client Key Exchange

- Server의 인증서를 받은 Client는 `Client Random Data`와 `Server Random Data를` 사용하여 `Pre master secret`이라는 키를 생성한다. 이것은 대칭키에 해당하는 부분이다.

    ![https://yongineer.duckdns.org/security/10.png](https://yongineer.duckdns.org/security/10.png)

    이때 앞서 언급한 `diffie-hellman`을 이용해 키를 교환하는 것을 확인할 수 있다.

### 5. Key Generation

- Client와 Server모두 암, 복호화 할때 사용할 대칭키를 성공적으로 공유했다는 메시지를 주고 받는다.

### 6. Cipher Spec Exchange (Server / Client)

- 양측 모두 메시지를 확인한 이후로 통신에 사용되는 모든 메시지는 `Handshake`에서 결정된 알고리즘과 키를 이용하여 암호화 하겠다는 메시지를 주고 받는다.

    ![https://yongineer.duckdns.org/security/11.png](https://yongineer.duckdns.org/security/11.png)

    Server

    ![https://yongineer.duckdns.org/security/12.png](https://yongineer.duckdns.org/security/12.png)

    Client

### 7. Application Data (Server / Client)

- 양측 모두 TLS로 암호화된 통신을 하는것을 확인할 수 있다.

    ![https://yongineer.duckdns.org/security/13.png](https://yongineer.duckdns.org/security/13.png)

    Client

    ![https://yongineer.duckdns.org/security/14.png](https://yongineer.duckdns.org/security/14.png)

    Server
