---
title: TIL.31) Docker?
date: 2020-08-05 17:08:92
category: Docker
thumbnail: './images/docker.png'
draft: false
---

![](./images/docker.png)

Docker는 가상화 기술이다. 가상화 컨테이너에 application 배포를 자동화 시켜주는 오픈소스 엔진이다.

컨테이너 가상화 실행 환경 위에 어플리케이션 배포 엔진을 더함 으로 사용자의 코드를 어디서든 빠르고 가볍게 실행 시킬 수 있는 기술을 제공한다. 이는 요즘 널리 사용되는 Micro Service Architecture와 CI/CD와 아주 잘 조화되어 각광을 받게된다.

실제로 Docker는 한 컨테이너 당 하나의 어플리케이션이나 프로세스를 실행하는 것을 권한다.

# Hypercisor 가상화 VS Container 가상화

![https://yongineer.duckdns.org/docker/til30_1.png](https://yongineer.duckdns.org/docker/til30_1.png)

## Hypervisor

- 물리적인 서버에서 하나 혹은 그 이상의 독립적인 운영체제가 돌아가는 구조
- 즉, 물리적 서버의 OS위에 여러 다른 독립적인 OS가 가상적으로 돌아가는 구조이다.
- 각각의 OS는 서로에 대해서 알지 못하며 Base OS도 알지 못한다.
- 이러한 가상화의 장점은 물리적 서버의 리소스를 더 효율적으로 사용할 수 있다는 것에 있다.
- 단점은 기술적으로 너무 무겁다는 것이다. 독립적인 OS를 실행시키는 것이기 때문에 부팅 시간이 길 수 밖에 없으며 리소스를 많이 차지할 수 밖에 없다.

## Container

- OS의 커널 위의 유저 공간에서 실행된다.
- 완전히 독립적인 운영체제를 가상화 하는 것이 아니라 독립적인 유저 공간을 가상화 한다고 생각하면 쉽다.
- 하나의 호스트 서버에서 여러 독립적인 유저 스페이스 인스턴스들을 가상적으로 실행할 수 있게 되는 것
- Hypervisor보다 훨씬 가볍기 때문에 빠르고 쉽게 독립적인 가상 환경을 실행시킬 수 있다.
- Docker image만 있으면 어디서든 쉽고 빠르게 테스트 환경, 샌드박스 환경 및 프로덕션 배포를 할 수 있다.
- 단점은 독립적인 OS가 아닌 유저 공간 가상화를 하는 형태이다 보니 운영체제가 전혀 틀린 호스트에서는 실행을 시킬수가 없다. 예를들어 Windows를 Linux 호스트에서 실행시킬수 없다.
- 완전히 독립적인 운영체제 가상화가 아니다 보니 보안적인 측면에서 Hypervisor보다 약할 수 밖에 없다.

# Docker 구조

## Docker client와 Server

- 클라이언트가 서버에 명령을 전달하고 서버가 실행시키는 구조
- `docker binary`커맨드가 클라이언트
- `dockerd`가 `docker deamon`혹은 `docker engine`이다.
- `docker engine`과 interact하기 위한 `RESTful API`도 제공된다.
- 클라이언트와 서버는 동일한 호스트 안에서 운영 될 수 있으며 서로 다른 호스트에서 운영 될수도 있다.

### Docker Image

- Docker의 Life cycle에서 Docker Image는 `Build`에 해당한다.
- 도커 컨테이너에서 실행 시키고 싶은 어플리케이션을 도커 이미지로 빌드해서 실행 시키게된다.

### Docker registries

- 도커 이미지를 저장하는 repository
- `dockerhub`같은 docker registries에 저장한다고 생각하면 된다.
- `public registry`가 있고 `private registry`가 있다.

### Docker containers

- 컨테이너에서 도커 이미지가 실행된다.
- 도커 이미지를 실행시키는 가상화 공간이다.
- 컨테이너는 하나 혹은 그 이상의 프로세스를 실행 시킬수 있지만 하나의 프로세스만 실행시키는 것을 권장한다.

### Docker Compose And Docker Swarm

- 여러 도커 컨테이너들로 이루어진 스택이나 클러스터를 관리하는 서비스

**Docker Compose**

- 여러 도커 컨테이너들을 모아서 종합적인 어플리케이션 스택을 정의하고 운영할 수 있도록 해주는 서비스
- Compose 파일을 사용하여 전체적인 어플리케이션 서비스를 설정한 후 어플리케이션을 이루고 있는 각각의 컨테이너들을 따로 실행 시킬 필요 없이 한번에 생성하고 실행할 수 있도록 해준다.

**Docker Swarm**

- 도커 컨테이들로 이루어진 클러스터를 관리할 수 있도록 해주는 서비스

## Docker VS Configuration Management Tools

- 도커를 사용함으로서 많은 CM부분이 해결되는것은 맞지만 그래도 여전히 CM의 역활이 남아있다.
- 예를 들어 도커를 사용하기 위해서는 도커가 호스트에 설치되어 있어야 한다. 이러한 부분은 CM tool을 사용하여 관리 할 수있다.

> ⚠️ CM Tools
>
>- Chef나 Puppet, Ansible 같은 Tool이 있다.
