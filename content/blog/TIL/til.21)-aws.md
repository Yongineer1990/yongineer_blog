---
title: TIL.21) AWS
date: 2020-07-27 20:54:08
category: TIL 
thumbnail: './images/aws.png' 
draft: false
---

![](./images/aws.png)

Amazon Web Service는 유저가 직접 서버를 구입하고 설치할 필요 없이 AWS상에서 클릭 몇 번으로 서버를 구축하고 사용할 수 있게 해주는 서비스

# AWS 필수 개념

## EC2

- Elastic Compute Cloud
- AWS상에서 사용하는 Server, EC2 서버에 API를 배포하게 된다.
- 다양한 사양의 옵션을 제공하며 당연하게도 사양이 좋을수록 비싸다.

## 보안그룹 (Secuirty Group)

- 가상 방화벽
- 보안그룹 설정을 해줘야 EC2 인스턴스에 HTTP와 SSH 접속이 가능하다.

## RDS

- Database 서비스
- 사용자가 직접 서버를 생성해서 데이터베이스를 설치하고 설정하고 관리 하지 않아도 된다.
- 사용자가 직접 데이터베이스를 설치하고 운영하는 것 보다 RDS를 사용하는 것이 더 저렴하다.

## Loand Balancer

- HTTP 요청들을 여러 서버에 분산할 때 사용된다.
- HTTP 요청이 많을때는 서버 하나만으로 모두 처리하기 힘들기 때문에 서버 수를 늘리는것이 일반적이다. 그러므로 여러 서버를 실행하고 로드발란서가 HTTP요청들을 서버들에 분산 해주는 형태로 시스템이 구성된다.

## Route 53

- DNS 서비스
- API시스템을 실제 도메인과 연결 시켜주는 기능을 제공한다.

## AWS S3

- Simple Storage Service
- 파일을 쉽게 저장할 수 있는 공간을 제공하는 서비스
- 파일 저장 뿐만아니라 파일마다 고유 주소를 부여해주기 때문에 S3에 저장한 파일을 웹상에서 쉽게 읽어들일수 있다.
