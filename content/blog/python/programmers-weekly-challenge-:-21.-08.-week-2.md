---
title: Programmers Weekly Challenge - 21. 08. Week 2
date: 2021-08-14 02:08:38
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# 문제

### **설명**

대학 교수인 당신은, 상호평가를 통하여 학생들이 제출한 과제물에 학점을 부여하려고 합니다. 아래는 0번부터 4번까지 번호가 매겨진 5명의 학생들이 자신과 다른 학생의 과제를 평가한 점수표입니다.
<div class="page-body"><table class="collection-content"><thead><tr><th>No.</th><th>0</th><th>1</th><th>2</th><th>3</th><th>4</th></tr></thead><tbody><tr id="fd328b50-14ae-4d8b-804e-2995a8201936"><td class="cell-title"><a href="https://www.notion.so/0-fd328b5014ae4d8b804e2995a8201936">0</a></td><td class="cell-rqwu"><del><strong>100</strong></del></td><td class="cell-Qg:<">90</td><td class="cell-l<YY">98</td><td class="cell-nQmi">88</td><td class="cell-IHBv">65</td></tr><tr id="8e5edaa6-0a8b-4a5e-b1a5-07e71b99a2b9"><td class="cell-title"><a href="https://www.notion.so/1-8e5edaa60a8b4a5eb1a507e71b99a2b9">1</a></td><td class="cell-rqwu">50</td><td class="cell-Qg:<"><del><strong>45</strong></del></td><td class="cell-l<YY">99</td><td class="cell-nQmi">85</td><td class="cell-IHBv">77</td></tr><tr id="416239b2-7344-4a2c-b39b-a87e9db765a1"><td class="cell-title"><a href="https://www.notion.so/2-416239b273444a2cb39ba87e9db765a1">2</a></td><td class="cell-rqwu">47</td><td class="cell-Qg:<">88</td><td class="cell-l<YY"><strong>95</strong></td><td class="cell-nQmi">80</td><td class="cell-IHBv">67</td></tr><tr id="ae715a5f-7b25-411d-a0ce-74e81305f8f6"><td class="cell-title"><a href="https://www.notion.so/3-ae715a5f7b25411da0ce74e81305f8f6">3</a></td><td class="cell-rqwu">61</td><td class="cell-Qg:<">57</td><td class="cell-l<YY">100</td><td class="cell-nQmi"><strong>80</strong></td><td class="cell-IHBv">65</td></tr><tr id="c0761981-cfa2-4f14-9c64-664377679320"><td class="cell-title"><a href="https://www.notion.so/4-c0761981cfa24f149c64664377679320">4</a></td><td class="cell-rqwu">24</td><td class="cell-Qg:<">90</td><td class="cell-l<YY">94</td><td class="cell-nQmi">75</td><td class="cell-IHBv"><strong>65</strong></td></tr><tr id="1f8f1833-70d3-42f9-afc9-a74fc09c3124"><td class="cell-title"><a href="https://www.notion.so/1f8f183370d342f9afc9a74fc09c3124">평균</a></td><td class="cell-rqwu">45.5</td><td class="cell-Qg:<">81.25</td><td class="cell-l<YY">97.2</td><td class="cell-nQmi">81.6</td><td class="cell-IHBv">67.8</td></tr><tr id="c319c5e2-613c-495b-ba7b-b06a88714c91"><td class="cell-title"><a href="https://www.notion.so/c319c5e2613c495bba7bb06a88714c91">학점</a></td><td class="cell-rqwu">F</td><td class="cell-Qg:<">B</td><td class="cell-l<YY">A</td><td class="cell-nQmi">B</td><td class="cell-IHBv">D</td></tr></tbody></table></div>
위의 점수표에서, i행 j열의 값은 i번 학생이 평가한 j번 학생의 과제 점수입니다.

- 0번 학생이 평가한 점수는 0번 `행`에담긴 [**100**, 90, 98, 88, 65]입니다.
    - 0번 학생은 자기 자신에게 100점, 1번 학생에게 90점, 2번 학생에게 98점, 3번 학생에게 88점, 4번 학생에게 65점을 부여했습니다.
- 2번 학생이 평가한 점수는 2번 `행`에담긴 [47, 88, **95**, 80, 67]입니다.
    - 2번 학생은 0번 학생에게 47점, 1번 학생에게 88점, 자기 자신에게 95점, 3번 학생에게 80점, 4번 학생에게 67점을 부여했습니다.

당신은 각 학생들이 받은 점수의 평균을 구하여, 기준에 따라 학점을 부여하려고 합니다.만약, 학생들이 자기 자신을 평가한 점수가 **유일한 최고점** 또는 **유일한 최저점**이라면 그 점수는 제외하고 평균을 구합니다.

- 0번 학생이 받은 점수는 0번 `열`에 담긴 [`100`, 50, 47, 61, 24]입니다. 자기 자신을 평가한 100점은 자신이 받은 점수 중에서 유일한 최고점이므로, 평균을 구할 때 제외합니다.
    - 0번 학생의 평균 점수는 (50+47+61+24) / 4 = 45.5입니다.
- 4번 학생이 받은 점수는 4번 `열`에 담긴 [65, 77, 67, 65, `65`]입니다. 자기 자신을 평가한 65점은 자신이 받은 점수 중에서 최저점이지만 같은 점수가 2개 더 있으므로, 유일한 최저점이 아닙니다. 따라서, 평균을 구할 때 제외하지 않습니다.
    - 4번 학생의 평균 점수는 (65+77+67+65+65) / 5 = 67.8입니다.

제외할 점수는 제외하고 평균을 구한 후, 아래 기준에 따라 학점을 부여합니다.
<div class="page-body"><table class="collection-content"><thead><tr><th>평균</th><th>학점</th></tr></thead><tbody><tr id="9c091d37-f81b-4271-a0a0-32cbd4fa98ba"><td class="cell-title"><a href="https://www.notion.so/90-9c091d37f81b4271a0a032cbd4fa98ba">90점 이상</a></td><td class="cell-~g_;">A</td></tr><tr id="9f88c259-3652-4bd0-9083-afe3433b0c35"><td class="cell-title"><a href="https://www.notion.so/80-90-9f88c25936524bd09083afe3433b0c35">80점 이상 90점 미만</a></td><td class="cell-~g_;">B</td></tr><tr id="0b420f50-31ab-494c-a784-6d1423c5a819"><td class="cell-title"><a href="https://www.notion.so/70-80-0b420f5031ab494ca7846d1423c5a819">70점 이상 80점 미만</a></td><td class="cell-~g_;">C</td></tr><tr id="159fd8b3-d147-49e2-8265-26816ee0ca47"><td class="cell-title"><a href="https://www.notion.so/50-70-159fd8b3d14749e2826526816ee0ca47">50점 이상 70점 미만</a></td><td class="cell-~g_;">D</td></tr><tr id="78b39c47-319c-4def-ac83-77a8a25ebdb3"><td class="cell-title"><a href="https://www.notion.so/50-78b39c47319c4defac8377a8a25ebdb3">50점 미만</a></td><td class="cell-~g_;">E</td></tr></tbody></table></div>
학생들의 점수가 담긴 정수형 2차원 배열 scores가 매개변수로 주어집니다. 이때, 학생들의 학점을 구하여 하나의 문자열로 만들어서 return 하도록 solution 함수를 완성해주세요.

### 제한사항

- 2 ≤ `scores`의 행의 길이(학생 수) ≤ 10
- `scores`의 열의 길이 = `scores`의 행의 길이
    - 즉, `scores`는 행과 열의 길이가 같은 2차원 배열입니다.
- 0 ≤ `scores`의 원소 ≤ 100
- return 값 형식
    - 0번 학생의 학점부터 차례대로 이어 붙인 하나의 문자열을 return 합니다.

### 입출력 예시
<div class="page-body"><table class="collection-content"><thead><tr><th>scores</th><th>result</th></tr></thead><tbody><tr id="7ef23a2c-1805-4db9-ae67-188726b3049e"><td class="cell-title"><a href="https://www.notion.so/100-90-98-88-65-50-45-99-85-77-47-88-95-80-67-61-57-100-80-65-24-90-94-75-65-7ef23a2c18054db9ae67188726b3049e">[[100,90,98,88,65],[50,45,99,85,77],[47,88,95,80,67],[61,57,100,80,65],[24,90,94,75,65]]</a></td><td class="cell-M\rN"><code>"FBABD"</code></td></tr><tr id="929e66e6-8e97-4f60-ae57-dbb09e008752"><td class="cell-title"><a href="https://www.notion.so/50-90-50-87-929e66e68e974f60ae57dbb09e008752">[[50,90],[50,87]]</a></td><td class="cell-M\rN"><code>"DA"</code></td></tr><tr id="a58d7d35-f1b7-46e4-bac8-e3c6fc8eeb57"><td class="cell-title"><a href="https://www.notion.so/70-49-90-68-50-38-73-31-100-a58d7d35f1b746e4bac8e3c6fc8eeb57">[[70,49,90],[68,50,38],[73,31,100]]</a></td><td class="cell-M\rN"><code>"CFD"</code></td></tr></tbody></table></div>
# 풀이

```python
import collections

def grade(score):
    if score >= 90:
        return 'A'
    elif score >= 80:
        return 'B'
    elif score >= 70:
        return 'C'
    elif score >= 50:
        return 'D'
    else:
        return 'F'

def solution(scores):
    avg = ''
    z = list(map(list, zip(*scores)))
    for i in range(0, len(z)):
        self_score = z[i][i]
        if self_score == min(z[i]) or self_score == max(z[i]):
            if collections.Counter(z[i])[self_score] == 1:
                z[i].remove(self_score)

        avg += grade(sum(z[i]) / len(z[i]))

    return avg
```

먼저 행열 전환부터 시켰다.

```python
z = list(map(list, zip(*scores)))
```

그 반복을 수행하면서 자기 자신에게 부여한 점수(`self_score`)를 가져와 만약 그 점수가 해당 학생의 점수 중 최대값 또는 최소값인지 확인하고 만약 최대 or 최소 값이라면 유일한 값이 아닌지 `collections.Counter()`를 통해 확인했다.

```python
for i in range(0, len(z)):
		self_score = z[i][i]
		if self_score == min(z[i]) or self_score == max(z[i]):
				if collections.Counter(z[i])[self_score] == 1:
						z[i].remove(self_score)
```

만약 조건에 부합한다면 해당 점수 리스트에서 자기 자신에게 부여한 점수를 점수 리스트에서 삭제하고 평균을 구해 `grade`함수에게 넘겨 준다.

이후 `grade`함수가 넘겨준 값들을 모두 연결시켜 리턴해 마무리 한다.
