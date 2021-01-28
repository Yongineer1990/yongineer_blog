---
title: gunicorn & Nginx를 사용하여 배포하기
date: 2020-10-05 02:10:45
category: AWS
thumbnail: '../TIL/images/aws.png'
draft: false
---

![](../TIL/images/aws.png)

# gunicorn

## 설치하기

```bash
$ pip install gunicorn
```

설치시 만약 가상환경을 사용한다면 가상환경 상에서 설치해줘야 한다.

## gunicorn을 사용하여 웹서버 실행해보기

gunicorn을 설치 한 후 gunicorn이 django web server를 실행 할 수 있는지 확인해 보자

```bash
$ gunicorn --bind 0.0.0.0:8000 [Project Name].wsgi:application
```

실행이 잘 되고 작성한 api가 잘 호출 된다면 종료한다.

## gunicorn 설정하기

```bash
$ vim /etc/systemd/system/gunicorn.service
```

시스템에 `gunicorn`을 등록하기 위해 `gunicorn.service`파일을 생성하여 아래의 내용을 입력한다.

```bash
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=[프로젝트 경로]
ExecStart=[gunicorn 설치 경로] --workers 3 --bind 0.0.0.0:8000 [프로젝트].wsgi:application

[Install]
WantedBy=multi-user.target
```

만약 가상환경을 통해 `gunicorn`을 설치했다면 `ExecStart`의 경로는 가상환경에서의 `gunicorn`설치 경로가 된다.

## gunicorn service 등록

```bash
$ sudo systemctl start gunicorn
$ sudo systemctl enable gunicorn
$ sudo systemctl status gunicorn.service
```

![https://yongineer.duckdns.org/aws/01.png](https://yongineer.duckdns.org/aws/01.png)

만약 제대로 실행 되었다면 다음과 같이 Active 상태임을 확인 할 수 있다.

# Nginx

## 설치하기

```bash
$ sudo apt-get update
$ sudo apt-get install nginx
```

## 설정 추가 하기

먼저 다음과 경로에 있는 `default`설정을 삭제 or 다른 경로에 백업한다. 여기서는 삭제로 진행하겠다.

```bash
$ sudo rm -f /etc/nginx/sites-enabled/default
$ sudo rm -f /etc/nginx/sites-available/default 
```

그리고 프로젝트에 맞는 설정 파일을 생성한다.

```bash
$ sudo vim /etc/nginx/sites-enabled/프로젝트이름
```

해당 파일에 다음과 같은 내용을 작성한다.

```bash
server {
    listen 80;
    server_name [IP주소];

    charset utf-8;

    location / {
        include proxy_params;
        proxy_pass http://[IP주소]:8000;

    }
}
```

이때 `ip`주소는 퍼블릭 IPv4 주소이다.

## Nginx 시작하기

```bash
$ sudo service nginx restart 
$ sudo service nginx status
```

![https://yongineer.duckdns.org/aws/02.png](https://yongineer.duckdns.org/aws/02.png)

`Nginx`역시 `service`에 등록하고 난 후 정상적으로 실행이 된다면 상태확인 시 `active`상태임을 확인 할 수 있다.

# API 호출해보기

이제 그동안 사용하던 `port`없이 `http`포트(80 또는 생략가능)로 작성한 API를 호출해보자.

![https://yongineer.duckdns.org/aws/03.png](https://yongineer.duckdns.org/aws/03.png)
