---
title: AWS 아키텍처의 기본 사항
date: 2022-08-26 00:08:34
category: aws
thumbnail: { thumbnailSrc }
draft: false
---

![](../TIL/images/aws.png)

# AWS API와 상호 작용 하는 방법


### AWS 관리 콘솔

- 가장 쉽게 AWs 서비스와 상호 작용을 시작할 수 있는 곳
- GUI 환경

### AWS CLI

- 명령줄 인터페이스로 AWS 서비스를 관리하는 도구
- Amazon Linux / Amazon Linux 2로 인스턴스를 배포한 경우 AWS CLI version 1이 기본적으로 설치되어 있음

### AWS Cloud Development Kit (CDK) & AWS Software Development Kit (SDK)

- 클라우드 인프라를 코드로 정의하고 프로비저닝하기 위한 소프트웨어 개발 프레임워크.

> 💡 **AWS CDK & SDK 차이점**
>
> TL;DR
>
> CDK : 인프라 생성 및 관리하는데 사용
>
> SDK : 애플리케이션을 개발하는데 사용
>
> ex) CDK를 사용하여 DynamoDB 테이블을 생성하고 SDK를 이용해 해당 테이블의 데이터를 읽고 쓴다.
>
>
> CDK는 Cloudformation을 python, js와 같은 프로그래밍 언어로 작성한다. 또한 코드와 Cloudformation 렌더링 사이에 중간 계층에 위치하여 원하는 작업을 이해하고 변환이 가능하다. 따라서, CDK는 배포 및 상태관리가 가능하다.
>
> SDK는 AWS API를 사용하기 위한 CLI를 프로그래밍 언어로 추상화한 라이브러리에 가깝다. AWS API는 Cloudformation을 지원하지 않고 배포 관리 및 상태관리를 지원하지 않기때문에 SDK도 마찬가지로 배포 및 상태관리는 불가능하다.
>
> 다만, SDK는 AWS 대부분의 서비스에 상호작용하기 위해 사용된다. 예를 들어 Python으로 작성된 애플리케이션에서 SQS 대기열의 메시지를 처리해야 하는 경우 Python SDK를 사용하여 SQS 메시지를 처리한다. CDK는 상태관리가 가능하기 때문에 인프라를 생성하거나 삭제하거나 관리하는데 사용하며 보통 AWS 인프라를 구축하는데 사용하는 Cloudformation을 프로그래밍 언어로 작성하는데 사용한다.

### CloudFormation

- AWS 리소스를 대규모로 배포하는 코드형 인프라 (laC) 자동화 플랫폼

# AWS 서비스의 형태


## 비관리형 서비스

![Untitled.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b2b6806b-5284-425e-af64-a0dec9de2cec/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220825%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220825T160451Z&X-Amz-Expires=86400&X-Amz-Signature=88b3f5e6ea3a6edeb2c11623abe3c5c14ba39f0b07e52355e87dd35879225689&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 대표적으로 EC2는 비관리형 서비스이다.
- EC2의 OS 설치 및 하드웨어 인프라 관리 (전력관리, 랙, 스택 관리)는 AWS에서 관리하지만
- 해당 EC2에 올라간 서비스에 대한 최적화 및 스케일링 관리, 고가용성, 백업, 패치와 같은 작업은 사용자가 관리해야 한다.
- ex) EC2 인스턴스를 생성하여 서비스 중인 MongoDB

## 관리형 서비스

![Untitled 1.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e6b60aa0-66d4-4a8c-85a9-851cbe5b14e5/Untitled_1.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220825%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220825T160555Z&X-Amz-Expires=86400&X-Amz-Signature=c7035d06ba7f596a44691dbba6168f2995e746d111f45b5b6307b89c6bf37079&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled%25201.png%22&x-id=GetObject)

- 관리형 서비스를 사용하면 AWS에서 인스턴스를 시작하고 유지보수까지 담당하여 처리해준다.
- 하지만 어떤 AWS 서비스를 사용하든 반드시 구성해야 하는 특정 보안 기능이 있으며 예를 들면 다음과 같은 보안 설정을 해줘야 한다.
    - AWS IAM 권한
    - 기본 데이터 베이스 암호 등 사용자 계정 및 보안 인증 정보
    - SSL/TLS
    - 사용자 활동 로깅
- 관리형 서비스를 사용하면 사용자는 애플리케이션 개발 및 최적화에만 집중할 수 있고 나머지는 AWS가 관리하여 관리포인트의 부담을 덜어주는 효과가 있다
- ex) RDS

## 서버리스 서비스

- 인프라를 관리할 필요 없이 애플리케이션과 서비스를 구축하고 실행하는 방식
- 애플리케이션은 여전히 서버에서 실행 되지만 모든 관리는 AWS에서 수행된다.
- 개발자는 클라우드나 온프레미스의 서버 또는 런타임 관리와 운영에 대해 신경쓰지 않고 핵심 제품에 집중할 수 있는 장점이 있다.
- ex) lambda

# AWS 인프라


![Untitled 2.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4ad622c0-9d65-46d3-ba2d-396fb520006d/Untitled_2.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220825%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220825T160623Z&X-Amz-Expires=86400&X-Amz-Signature=82084dad0b93aac636ba82f1f0862a6c8b0672dca2bb45465bd9061f24a9ca57&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled%25202.png%22&x-id=GetObject)

## 데이터 센터

- 서버가 위치한 물리적인 공간

## 가용영역 (AZ, Availability Zone)

- 하나 이상의 데이터 센터 그룹을 가용영역이라 한다.
- 가용영역은 특정 지리적 위치 내의 격리된 여러 영역이다.
- 인스턴스를 실행할때 직접 가용영역을 선택하거나 AWS가 대신 가용영역을 선택하도록 할 수 있다.
- 여러 가용영역에 인스턴스가 배포되고 인스턴스 하나에 장애가 발생하는 경우 다른 가용영역의 인스턴스가 요청을 처리 할 수 있도록 애플리케이션을 설계할 수 있다.

## 리전 (Region)

- 각 AWS 리전은 하나의 지리적 영역 내에서 여러개의 가용영역으로 구성된다.
- AWS의 리소스는 리전을 지정할 수 있으며 콘솔을 통해 보면 지정한 리전에 연결된 리소스만 표시되는 걸 알 수 있다. 이는 리전이 서로 격리 되어 있기 때문이며, AWS는 리전 간 자동으로 리소스를 복제하지 않는다.

### 리전 선택 방법

1. **거버넌스 및 법적 요구 사항**
    1. 법적 요구 사항을 고려하여 리전을 선택할수 있다.
2. 지연시간
    1. 고객과 가까운 리전을 선택한다.
3. 서비스 가용성
    1. 모든 AWS 서비스를 모든 리전에서 사용할 수 있는것은 아니다. 따라서 이용할 서비스를 지원하는 리전을 선택한다.
4. 비용
    1. 리전바다 비용이 다르므로 사용하려는 서비스의 요금제를 살펴보고 워크로드에 가장 적합한 결정을 해야한다.

## Local Zone

- 10ms 미만의 지연시간이 필요한 수요가 있는 애플리케이션에서 사용을 고려해볼 수 있다.
    - 미디어 컨텐츠 생성
    - 실시간 멀티플레이어 게임
    - 머신러닝 호스팅 및 훈련
    - AR & VR

## 엣지 로케이션

![Untitled 3.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/23511e05-f083-474f-b830-574920364143/Untitled_3.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220825%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220825T160644Z&X-Amz-Expires=86400&X-Amz-Signature=18bb13248884367d8343b55d6be4f401ae9ef5050e86f606289b7e4ea4274cfb&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled%25203.png%22&x-id=GetObject)

- 요청자에 가장 가까이 있는 지점이다.
- 전 세계 주요 도시에 엣지 로케이션이 위치해 있고 엣지 로케이션은 요청을 수신하고 더 빠른 전송을 위해 컨텐츠 사본을 캐싱한다.
- 최종 사용자에게 짧은 지연 시간으로 컨텐츠를 전송하기 위해서 사용
- ex) 서울 리전 S3에 저장한 비디오 컨텐츠를 남미 리전 고객에게 제공해야 할때

> 💡 **Cloudfront 에서의 엣지 로케이션 사용**
>
> 클라우드프론트는 엣지로케이션 및 엣지 캐시 서버로 구성된 PoP(Point of Presence) 위치의 글로벌 네트워크를 통해 컨텐츠를 전송하다.
> 이중 기본적으로 사용하는 리전 엣지 캐시는 엣지 로케이션에 유지할 필요 없는 컨텐츠가 있을 때 활용 된다.

### Local Zone VS 엣지 로케이션

![Untitled 4.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/87461673-1291-4c89-ba90-f208fa726af5/Untitled_4.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220825%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220825T160703Z&X-Amz-Expires=86400&X-Amz-Signature=470325b33bbdf9b5b237d45cd02146ee021e8b40931cd37af2fca698bf8c652e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled%25204.png%22&x-id=GetObject)

- AWS 서비스를 로컬 온디멘드 처럼 사용 : Local Zone
- 컨텐츠를 글로벌 유저에게 빠르게 전송 : 엣지 로케이션
