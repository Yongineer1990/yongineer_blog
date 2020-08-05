---
title: TIL.32) Docker 배포하기 (feat. Synology NAS)
date: 2020-08-06 02:08:02
category: Docker
thumbnail: './images/docker.png'
draft: false
---

![](./images/docker.png)

Django로 생성된 프로젝트를 Docker로 배포하는 튜토리얼이다. 다만 몇일전 부터 AWS의 Free Tier에 한계가 다다랐다는 압박 메일이 날라왔고 울며 겨자먹기로 보유하고 있던 시놀로지 나스에 도커를 올려보기로 했다.

![https://yongnas.iptime.org/docker/til31_3.png](https://yongnas.iptime.org/docker/til31_3.png)

*너무 무서운것...*

# Docker 설치하기

## Mac OS & Windows

[여기](https://www.docker.com/get-started)서 설치하자

## Linux

다음과 같은 명령으로 설치하자 이는 AWS의 EC2역시 리눅스 환경이라면 필요한 부분이다.

```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
apt-cache policy docker-ce
```

```bash
sudo apt install docker-ce
```

단, 한줄 한줄 명령어를 입력할것!

# 프로젝트 설정하기

## gunicorn 설치

```python
pip install gunicorn
```

- 생성한 어플리케이션이 `gunicorn`에서 백그라운드에서 실행될 예정이므로 해당 패키지를 설치해준다.

## requirements.txt 생성하기

```python
pip freeze > requirements.txt
```

- 프로젝트의 필요한 패키지들의 대한 정보를 담은 requirements.txt를 생성해준다.
- requirements.txt에 명시된 패키지들이 Docker 이미지에 설치 될 것이다.
- requirements.txt는 프로젝트의 root경로에 위치하여야 한다.

## Dockerfile 생성하기

```docker
# Base Docker지정
FROM python:3

# 프로젝트가 실행될 경로 지정
WORKDIR /usr/src/app

# requirements.txt 경로 지정 및 실행
COPY requirements.txt ./
RUN pip install -r requirements.txt

# 복사할 프로젝트 내용물 지정
COPY . .

# 어플리케이션이 실행될 포트 지정
EXPOSE PORT

# gunicorn 실행 설정
CMD ["gunicorn", "-b", "0.0.0.0:PORT", "chanel.wsgi:application"]
```

## (선택) 데이터베이스 서버 위치 변경

만약 데이터베이스 서버를 local환경에서 사용하고 있었다면 이를 AWS의 RDS나 나같이 Free Tier 경고가 무서워 NAS로 옮긴 상태라면 해당 경로로 지정해줘야 한다.

```python
DATABASES = {
     'default' : {
         'ENGINE': 'django.db.backends.mysql',
         'NAME': 'DB_name',
         'USER': 'User_name',
         'PASSWORD': 'User_password',
         'HOST': 'Database host address',
         'PORT': 'Database PORT',
     }
}
```

# Docker image 생성하기

## Build 하기

```bash
$ (sudo) docker build -t yongineer/chanel_backend:0.1.0 .
```

이때 유저명 (`yongineer`)이 다르다면 docker hub에 push되지 않으므로 반드시 도커 유저명을 명시해야 하며 해당 이미지의 버전 또한 반드시 명시되어야 한다.

빌드가 끝났다면 이미지가 생성 되었는지 확인한다.

```bash
$ (sudo) docker images
```

![https://yongnas.iptime.org/docker/til31_1.png](https://yongnas.iptime.org/docker/til31_1.png)

## 실행해보기

```bash
$ (sudo) docker run --name chanel -d -p PORT:PORT yongineer/chanel_backend:0.1.0
```

도커가 잘 올라갔는지 확인해보자.

```bash
$ (sudo) docker ps
```

![http://yongnas.iptime.org/docker/til31_2.png](http://yongnas.iptime.org/docker/til31_2.png)

만약 정상적으로 실행되지 않았다면 에러가 발생했을 것이다.

### 에러가 났다면?

일단 로그부터 확인해보자

```bash
$ (sudo) docker logs CONTAINER ID
```

> ⚠️ 모든 컨테이너 실행 내역 확인 하기
>
아래 명령을 실행하면 STATUS에 에러 코드가 기록되어 있다.
>
>    ```bash
>    $ (sudo) docker ps -a
>    ```

**내가 만난 에러코드**

Exited (126)

- docker Container 내부에서 Command를 실행하지 못할 경우 발생 (권한 문제, 접근 문제, 파일 문제, 옵션 문제 등등으로 발생 가능함)
- `wsgi`이름을 잘못 적어서 해당 에러가 발생했었다.

Exited (127) : docker Container 내부에서 Command를 발견하지 못하였을 경우 발생

- `gunicorn`설정을 잘못해 해당 에러가 발생했었다.

## 컨테이너 종료 및 삭제

### 컨테이너 종료

```bash
$ (sudo) docker stop CONTAINER ID
```

### 컨테이너 삭제

```bash
$ (sudo) docker rm CONTAINER ID
```

> ⚠️ 이미지 삭제
>
> ```bash
>$ (sudo) docker rmi IMAGE ID
>```

# Docker hub에 push하기

## LOGIN 하기

```bash
$ (sudo) docker login
```

- 도커에 로그인해야 도커허브에 생성된 이미지를 push할 수 있다.
- 아마 최초 로그인을 한다면 유저명과 패스워드를 물어볼 것이다.

## Docker hub에 push하기

```bash
$ (sudo) docker push yongineer/chanel_backend:0.1.0
```

작업이 완료 된 후 도커허브에 있는 자신의 Repository에 업로드 된 것을 확인할 수 있다.

![https://yongnas.iptime.org/docker/til31_4.png](https://yongnas.iptime.org/docker/til31_4.png)

> ⚠️ Docker hub는 Public repository는 무제한이지만 Private repository는 계정당 1개 무료로 제공된다.

# Synology NAS에서 Docker 실행하기

## Docker 설치하기

일단 위에서 언급한 Linux 설치 방법으로 설치할 수 있지만 시놀로지 패키지 센터에서 도커를 받아서 설치 할 수있다.

![https://yongnas.iptime.org/docker/til31_5.png](https://yongnas.iptime.org/docker/til31_5.png)

## Docker hub에서 이미지 pull 받기

설치가 완료 되었으면 시놀로지 나스에 ssh로 접근해 docker hub에 있는 이미지를 pull 받을 수 있는데 앞서 했던것 처럼 로그인이 선행되어야 한다.

**로그인**

```bash
$ (sudo) docker login
```

**pull**

```bash
$ (sudo) docker pull yongineer/chanel_backend:0.1.0
```

## Docker container 실행하기

실행 하기 앞서 애플리케이션이 실행될 컨테이너의 포트를 포트포워딩으로 열어줘야 한다.

```bash
$ (sudo) docker run --name chanel -d -p PORT:PORT yongineer/chanel_backend:0.1.0
```

정상적으로 실행이 됐다면 아까 언급한 `$ (sudo) docker ps`명령으로 확인 할 수 있고 DSM에서 도커 어플리케이션을 실행하면 다음과 같이 나오는 것을 볼 수 있다.

![https://yongnas.iptime.org/docker/til31_6.png](https://yongnas.iptime.org/docker/til31_6.png)

## Test 해보기

도커 컨테이너가 실행되었으면 요청과 응답이 잘 되는지 확인해보자

![https://yongnas.iptime.org/docker/til31_7.png](https://yongnas.iptime.org/docker/til31_7.png)

![https://yongnas.iptime.org/docker/til31_8.png](https://yongnas.iptime.org/docker/til31_8.png)

굿, 이제 AWS의 무서운 메일을 받지 않아도 될 거 같다 ...

나같은 경우 DB 서버 또한 NAS에 올려놨는데 이것도 기회가 되면 포스팅 해봐야겠다.
