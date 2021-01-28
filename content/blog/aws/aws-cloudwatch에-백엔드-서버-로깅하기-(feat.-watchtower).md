---
title: AWS CloudWatch에 백엔드 서버 로깅하기 (feat. watchtower)
date: 2020-11-25 01:11:98
category: AWS
thumbnail: ../TIL/images/aws.png
draft: false
---

![](../TIL/images/aws.png)

CloudWatch는 AWS에서 제공하는 모니터링 및 관찰 서비스이다. CloudWatch를 사용하여 서버의 비정상적인 동작을 감지하고 경보를 설정하고 로그와 지표를 시각화하고 자동화된 작업을 수행하고 문제를 해결하고 인사이트를 발견하여 애플리케이션을 원활하게 관리할 수 있다. 여기에서는 Django 웹 애플리케이션 로깅을 AWS CloudWatch에 연결하는 방법을 설명한다.

# Watchtower 사용하기

watchtower는 AWS CloudWatch Logs를 위한 오픈소스 로그 핸들러 이며 AWS로 쉽게 로그를 보낼 수 있게 도와주며 다음과 같이 패키지를 설치 한다.

```python
pip install watchtower
pip install boto3
```

django의 `settings.py`에 다음과 같이 code를 작성한다.

```python
import logging
from boto3.session import Session

AWS_ACCESS_KEY_ID = 'aws access key'
AWS_SECRET_ACCESS_KEY = 'aws secret access key'
AWS_REGION_NAME = '리전 이름 ex) 서울 : ap-northeast-2'
AWS_LOG_GROUP = '로그그룹 이름',
AWS_LOG_STREAM = '로그스트림 이름',
AWS_LOGGER_NAME = 'my-logger' # logger를 지정해준다

# logger
boto3_session = Session(
  aws_access_key_id=AWS_ACCESS_KEY_ID,
  aws_secret_access_key=AWS_SECRET_ACCESS_KEY,
  region_name=AWS_REGION_NAME
)

LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'formatters': {
        'aws': {
            # 여기에서 AWS에 대한 특정 포맷을 추가할 수 있다.
            # 포맷을 변경하려면 다음을 참고 한다.
            # https://stackoverflow.com/questions/533048/how-to-log-source-file-name-and-line-number-in-python/44401529
            'format': u"%(asctime)s [%(levelname)-8s] %(message)s [%(pathname)s:%(lineno)d]",
            'datefmt': "%Y-%m-%d %H:%M:%S"
        },
    },
    'handlers': {
        'watchtower': {
            'level': 'DEBUG',
            'class': 'watchtower.CloudWatchLogHandler',
                     'boto3_session': boto3_session,
                     'log_group': AWS_LOG_GROUP,
                     'stream_name': AWS_LOG_STREAM,
            'formatter': 'aws', # 앞에서 생성한 format
        },
    },
    'loggers': {
        AWS_LOGGER_NAME: {
            'level': 'DEBUG',
            'handlers': ['watchtower'],
            'propagate': False,
        },
        # 다른 로거들을 이 다음에 추가할 수 있다.
    },
}
```

여기서는 `AWS_ACCESS_KEY_ID`와 `AWS_SECRET_ACCESS_KEY`를 통해 자격증명을 받았지만 IAM계정에 `CloudWatchLogs`관련 권한을 설정하고 이를 `boto3`가 인스턴스 메타데이터에서 자격 증명을 수집하도록 요청할 수 도 있다. 한편 `AWS_ACCESS_KEY_ID`와 `AWS_SECRET_ACCESS_KEY`는 IAM계정 생성시 발급된다.

## Log group

![https://yongineer.duckdns.org/aws/04.png](https://yongineer.duckdns.org/aws/04.png)

로그 그룹은 CloudWatch → 로그 그룹에서 생성가능하며 이때 생성한 이름을 `settings.py`의 `AWS_LOG_GROUP`에 추가해준다. 

## Log stream

![https://yongineer.duckdns.org/aws/05.png](https://yongineer.duckdns.org/aws/05.png)

로그 스트림은 CloudWatch → 생성한 로그 그룹 → 로그 스트림 생성에서 만들 수 있으며 이때 지정한 이름을 `settings.py`의 `AWS_LOG_STREAM`에 추가해준다.

또는 만약 로그 스트림을 생성하지 않은 상태라면 `settings.py`의 `AWS_LOG_STREAM`에서 지정된 이름으로 자동으로 로그 스트림이 생성된다.

# logger를 추가하기

```python
import logging

logger = logging.getLogger('my-logger')

def add (a, b):
	try:
		return int(a) + int(b)
	except:
		logger.error('test!') # CloudWatch로 전송되는 로그
```

위와 같이 테스트 코드를 작성하였으면 함수의 파라미터로 `1`과 `int`가 아닌`test`를 전달하여 고의로 에러를 발생 시켜 보자.

## CloudWatch Log 확인하기

![https://yongineer.duckdns.org/aws/06.png](https://yongineer.duckdns.org/aws/06.png)

이제 CloudWatch에서 생성한 (또는 생성된) 로그스트림에서 앞서 작성한 테스트 코드의 에러 로그를 확인 할 수 있다.
