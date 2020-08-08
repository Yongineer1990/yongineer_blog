---
title: 2차 프로젝트 후기 (Kartrider TMI)
date: 2020-07-27 20:58:22
category: Project Review
thumbnail: './images/kart_tmi.png'
draft: false
---


위코드 부트캠프에서 2차 프로젝트를 2주간 진행하였고 저는 **백엔드 부분을 담당**했습니다.

# wekart 소개

국내 최대 게임회사인 넥슨의 오랜기간 사랑받는 캐쥬얼 레이싱 장르인 카트라이더의 전적 정보를 제공하는 [Kartrider TMI](https://tmi.nexon.com/kart)를 클론하는 프로젝트입니다. 공식 홈페이지와는 다르게 소셜 로그인 기능이 없어 클론하지 않고 직접 개발하였으며 추가적으로 소셜계정과 실제 카트라이더 유저의 계정을 연동하는 기능도 추가하였습니다. 개발 인원은 총 5명이며 백엔드 3명 프론트엔드 2명으로 구성되었으며 여담으로 게임회사 출신이 저 포함 3명이나 되는 팀이었습니다.

- [Backend github](https://github.com/wecode-bootcamp-korea/9-5dragon-backend)
- [Frontend github](https://github.com/wecode-bootcamp-korea/9-5dragon-frontend)

## Demo
[![](https://images.velog.io/images/yongineer1990/post/87664e61-d90c-469b-8678-24f42405b1e6/image.png)*동영상 보기*](https://youtu.be/Av_p4sCT4Wg)

## 적용 기술 (Backend)

- Python
- Django
- Beautifulsoup
- Selenium
- Requests
- Pandas
- Crontab
- Design Pattern : Abstract factory
- Bcrypt
- JWT
- Social Login : KAKAO
- Mysql
- CORS headers
- AWS : EC2, RDS, S3
- Docker
- Git Rebase

## 담당했던 부분

- Trello를 활용한 Task 관리
- DB Modeling 및 ERD, Django Model작성
- Pandas를 이용한 통계 데이터 도출
- Social Login (KAKAO) 구현
- 로그인시 JWT Token 발급 기능 구현
- 로그인 상태 체크를 위한 JWT Token 검증 기능 구현
- Social User와 Game User 연결 기능 구현
- Ranking 상세 페이지 정보를 GET 방식으로 전송하는 기능 구현
- Comment 기능 구현
- Pageview 기능 구현
- AWS EC2, RDS, S3 활용한 배포
- Docker를 활용한 배포
- Git Rebase를 이용한 Commit 관리

## GOOD

- 1차 프로젝트에 비해 사용한 기술이 풍부하였고 특히 데이터 분석 라이브러리인 Pandas를 다뤄 볼 수 있는 아주 좋은 기회였습니다.
- 팀원간 커뮤니케이션이 아주 잘되어 조급하지 않고 즐겁게 프로젝트를 진행 하였는데 체감상 1차 프로젝트 때보다 쉬엄쉬엄 했다고 느꼈는데 지나고보니 진행도가 쭉쭉 올라가있는 커뮤니케이션의 힘을 느낄 수 있었습니다.
- Unit Test, Git Rebase등 처음 접하는 기술들에 어려움을 겪었으나 차근차근 공부해가며 능숙하진 않아도 익숙하게 사용할 수 있는 수준에 올라왔다고 생각합니다.
- 업체에서 제공하는 API를 두개나 써봤고(판교N사, 제주 D사) 다양한 경험을 해봤습니다 어떤 API가 뛰어난지는 노코멘트하겠습니다.

## BAD

- 판교 N사가 제공하는 API가 있어 모델링에 큰 시간을 투자하지 않고 바로 데이터 분석에 들어갔는데 View 작성 시 모델링에 약간의 실수들이 있어 수정하였습니다.
- (커뮤니케이션이 너무 잘되어) 프로젝트 관리 툴인 Trello와 Postman API Documentation 활용을 잘 하지 못하였습니다.
- 개인적으론 Pandas를 사용하여 통계데이터를 Python에서 연산하는 것 보다 빠르게 도출하였으나 그럼에도 퍼포먼스적인 부분을 충분하게 고려하지 못했다고 생각합니다.

## 개선점

- Unit Test 수행시 더욱 다양한 예외처리 케이스를 생각해보고 View 작성 과정에서 해당 예외처리를 모두 포함하여 작성해야겠습니다.
- 커뮤니케이션의 원활 유무와 상관없이 프로젝트 관리 툴을 활용하는것은 히스토리를 남기는것과 같기 때문에 적극 활용하도록 습관을 길러야겠다고 생각했습니다.

# 기록하고 싶은 코드

## mark_apistat_uploader

```python
## Abstract Factory
class NXApiStat(metaclass = ABCMeta):
    def __init__(self, access_id, match_types, start_date, end_date):
        self.access_id = access_id
        self.match_types = match_types
        self.start_date = start_date
        self.end_date = end_date
        self.authorization_key = '-.-.-' #보안상 API Key는 비공개 처리합니다.
        self.current_URL = f"https://api.nexon.co.kr/kart/v1.0/users/{self.access_id}/matches?start_date={self.start_date}&end_date={self.end_date}&offset={offset}&limit={limit}&match_types={self.match_types}"
        self.limit500_URL = f"https://api.nexon.co.kr/kart/v1.0/users/{self.access_id}/matches?start_date={self.start_date}&end_date={self.end_date}&offset={offset}&limit={limit}&match_types={self.match_types}"

    def _api_to_df(self, url):
        # API Call
        req_data = requests.get(url, headers={'authorization': self.authorization_key}).text
        data = json.loads(req_data)

        # Get dfs
        matches_df = pd.DataFrame(data['matches'][0]['matches'])
        player_df = pd.json_normalize(matches_df['player'])

        # preprocess 1) join, replace empty value, convert (numeric/date)type
        df = pd.merge(matches_df, player_df, how='left', left_index=True, right_index=True)
        df = df.replace('', np.nan, regex=True)
        df['startTime'] = pd.to_datetime(df['startTime']) # format='%Y-%m-%d'
        to_num_cols = ['matchTime', 'matchWin', 'matchRetired', 'matchRank']
        df[to_num_cols] = df[to_num_cols].apply(pd.to_numeric, errors='coerce', axis=1)

        # preprocess 2) convert retired log: if matchRank == 99 then matchTime set to 0
        mask = (df['matchRank'] == 99)
        df.loc[mask, 'matchTime'] = 0

        # preprocess 3) drop na columns
        df = df.dropna(subset=['kart', 'matchTime', 'matchWin', 'matchRetired', 'matchRank'])

        return df

    @abstractmethod
    def summary_stat(self):
        pass

## RankMain(first_day_of_month to limit 500)
class RankMainRecord_Cumul(NXApiStat):
    def __init__(self, access_id, match_types, start_date = first_day_of_month, end_date = today):
        super().__init__(access_id, match_types, start_date, end_date)
        print(self.access_id, self.limit500_URL)

    def summary_stat(self):
        df = self._api_to_df(url = self.limit500_URL) # Get Cumul Records

        # [메인순위] (승률 / 리타이어율)
        # [종합전적] (전 / 승 / 패 / 승률 / 완주율 / 리타이어율)
        play_cnt = df['matchRank'].count()

        rank_df = df['matchRank'].value_counts().reset_index()
        win_mask = (rank_df['index'] == 1)
        retire_mask = (rank_df['index'] == 99)

        win_cnt = rank_df[win_mask].matchRank.item()

        win_ratio = round(win_cnt/play_cnt, 4)

        if len(rank_df[retire_mask].matchRank) != 0:
            retire_cnt = rank_df[retire_mask].matchRank.item()
        else:
            retire_cnt = 0
        retire_ratio = round(retire_cnt/play_cnt, 4)

        # [종합전적] 캐릭터이름
        character = df['character_x'].head(1).item()

        # [메인순위] (누적포인트)
        score_df = pd.DataFrame(
            {
                'score' : [10,7,5,4,3,1,0,-1,-5]
            }, index = [1, 2, 3, 4, 5, 6, 7, 8, 99]
        )
        user_rankcount = df['matchRank'].value_counts()
        result = pd.merge(user_rankcount, score_df, how='inner', left_index=True, right_index=True)
        result['Rank_Mul_score'] = result['matchRank'] * result['score']
        point_cumul = result['Rank_Mul_score'].sum()

        return play_cnt, win_cnt, win_ratio, retire_ratio, point_cumul, character

## RankMain(yeseterday)
class RankMainRecord_Recent(NXApiStat):
    def __init__(self, access_id, match_types, start_date = bf_yesterday, end_date = today):
        super().__init__(access_id, match_types, start_date, end_date)
        print(self.access_id, self.limit500_URL)

    def summary_stat(self):
        df = self._api_to_df(url = self.current_URL) # Get Recent Records

        # [메인순위] (주행 횟수) / 승률 / 리타이어율 / 누적포인트 / 포인트 변화 / {랭크 변화 / 평균 순위}
        play_count_day1 = df['matchRank'].count()

        # [메인순위] 주행 횟수 / 승률 / 리타이어율 / 누적포인트 / (포인트변화) / {랭크 변화 / 평균 순위}
        score_df = pd.DataFrame(
            {
                'score' : [10,7,5,4,3,1,0,-1,-5]
            }, index = [1, 2, 3, 4, 5, 6, 7, 8, 99]
        )
        user_rankcount = df['matchRank'].value_counts()
        result = pd.merge(user_rankcount, score_df, how='inner', left_index=True, right_index=True)
        result['Rank_Mul_score'] = result['matchRank'] * result['score']
        point_new_day1 = result['Rank_Mul_score'].sum()

        return play_count_day1, point_new_day1
```

판교 N사에서 제공하는 API에서 Raw Data를 가져와 Pandas를 통해 통계 데이터로 가공하는 과정입니다. Design Pattern은 Abstract Factory 방식을 적용하였습니다. Pandas를 이용한 통계 데이터 가공은 제가 작성하였으나 디자인 패턴을 적용한 리팩토링은 제가 하지 않아 관련하여 더 공부 하고 싶습니다.

## Social Login (KAKAO)

```python
# Views.py
class SocialLoginView(View):
    def post(self, request):
        try:
            kakao_token         = request.headers['Authorization']
            req_kakao_profile   = requests.get(KAKAO_API,headers = {
                'Authorization' : f'Bearer {kakao_token}'
            })
            profile         = req_kakao_profile.json()
            kakao_email     = profile['kakao_account']['email']
            kakao_id        = profile['properties']['nickname']
            kakao_picture   = profile['properties']['profile_image']

            if User.objects.filter(email = kakao_email).exists():
                token = jwt.encode({'email' : kakao_email}, SECRET_KEY, algorithm=ALGORITHM).decode('utf-8')
                return JsonResponse({
                    'access_token'      : token,
                    'nickname'          : kakao_id,
                    'profile_image'     : kakao_picture,
                    'email'             : kakao_email
                }, status=200)

            User(
                email           = kakao_email,
                kakao_id        = kakao_id,
                picture         = kakao_picture,
            ).save()

            token = jwt.encode({'email' : kakao_email}, SECRET_KEY, algorithm=ALGORITHM)
            token = token.decode('utf-8')

            return JsonResponse({
                'access_token'      : token,
                'nickname'          : kakao_id,
                'profile_image'     : kakao_picture,
                'email'             : kakao_email
            }, status=200)

        except KeyError:
                return JsonResponse({'Message' : 'INVALID_KEYS'}, status=400)
```

```python
# tests.py

class SocialLoginTest(TestCase):
    def setUp(self):
        User.objects.create(
            email           = 'test@test.com',
            kakao_id        = 'test',
            picture         = 'http://k.kakaocdn.net/dn/5w8QS/btqCrOP9kx6/XcCLekEj4OrEbCncK7CLiK/img_640x640.jpg'
        )
    def tearDown(self):
        User.objects.filter(email='test@test.com').delete()

    @patch('user.views.requests')
    def test_login_pass(self, mocked_request):

        class MockedResponse:
            def json(self):
                return {
                    'kakao_account' : {
                        'email' : 'test@test.com'
                    },
                    'properties' : {
                        'nickname'      : 'test',
                        'profile_image' : 'http://k.kakaocdn.net/dn/5w8QS/btqCrOP9kx6/XcCLekEj4OrEbCncK7CLiK/img_640x640.jpg'
                    }
                }

        mocked_request.get = MagicMock(return_value = MockedResponse())

        kakao_id = MockedResponse().json()['properties']['nickname']
        kakao_picture = MockedResponse().json()['properties']['profile_image']
        kakao_email = MockedResponse().json()['kakao_account']['email']

        client  = Client()
        headers = {
            'HTTP_Authorization'    : '',
            'content_type'          : 'application/json'
        }
        response = client.post('/user/login', **headers)
        token = response.json()['access_token']

        self.assertEqual(response.json(), {
            'access_token'  : token,
            'nickname'      : kakao_id,
            'profile_image' : kakao_picture,
            'email'         : kakao_email
        })
        self.assertEqual(response.status_code, 200)

    @patch('user.views.requests')
    def test_login_fail(self, mocked_request):

        class MockedResponse:
            def json(self):
                return {
                    'account' : {
                        'email' : 'test@test.com'
                    },
                    'properties' : {
                        'nickname'  : 'test',
                        'image'     : 'http://k.kakaocdn.net/dn/5w8QS/btqCrOP9kx6/XcCLekEj4OrEbCncK7CLiK/img_640x640.jpg'
                    }
                }

        mocked_request.get = MagicMock(return_value = MockedResponse())

        client  = Client()
        headers = {
            'HTTP_Authorization'    : '',
            'content_type'          : 'application/json'
        }
        response = client.post('/user/login', **headers)

        self.assertEqual(response.json(),{
            'Message' : 'INVALID_KEYS'
        })
        self.assertEqual(response.status_code, 400)
```

Kakao를 통한 Social Login 기능을하는 View와 Unit Test입니다.

소셜로그인의 경우 현재 다양한 서비스에서 사용되는 기술이므로 더 공부해볼만한 가치가 있다고 생각하며 더 효율적인 코드설계는 없는지 공부해봐야겠습니다.

Unit Test의 경우 일단 이 소셜로그인 과정의 Unit Test를 수행하는 것 자체가 너무 큰 허들이어서 선배님들 붙잡고 한줄한줄 print 찍어가며 공부했던 기억이 너무 강렬하여 반드시 기억하지 않으면 선배님들에게 죄짓는거 같고 그럼에도 불구하고 Pass case와 Fail case만 고려하여 예외처리는 생각하지 않아 소셜 로그인 관련 예외처리 케이스에 대해 더 깊게 고민해보고자 합니다.

### Bonus

전설로 기억될 저희 백엔드 팀의 프레젠테이션을 소개하지 않는것은 도리가 아니라고 생각됩니다 여기까지 읽어주신 분들은 이 영상도 꼭 봐주셨으면 좋겠습니다.
[![](https://images.velog.io/images/yongineer1990/post/2de3f743-342a-4ce9-8649-0edb25a54adc/image.png)*동영상 보기*](https://youtu.be/FyRGCjlBLo0)
