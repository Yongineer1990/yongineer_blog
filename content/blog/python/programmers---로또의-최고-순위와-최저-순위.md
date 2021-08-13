---
title: Programmers - 로또의 최고 순위와 최저 순위
date: 2021-08-14 03:08:64
category: Python
thumbnail: { thumbnailSrc }
draft: false
---

![](../TIL/images/python.png)

# 문제

### **설명**

`로또 6/45`(이하 '로또'로 표기)는 1부터 45까지의 숫자 중 6개를 찍어서 맞히는 대표적인 복권입니다. 아래는 로또의 순위를 정하는 방식입니다.
<div class="page-body"><table class="collection-content"><thead><tr><th>순위</th><th>당첨 내용</th></tr></thead><tbody><tr id="e62e888d-8d6b-4244-98da-7fcd3ebef147"><td class="cell-title"><a href="https://www.notion.so/1-e62e888d8d6b424498da7fcd3ebef147">1</a></td><td class="cell-E{uN">6개 번호가 모두 일치</td></tr><tr id="9de1d2b0-c120-48e6-8de4-bc8a6d1d30cc"><td class="cell-title"><a href="https://www.notion.so/2-9de1d2b0c12048e68de4bc8a6d1d30cc">2</a></td><td class="cell-E{uN">5개 번호가 일치</td></tr><tr id="2cc3e68f-2845-4bc2-9fd3-f2e5f929e819"><td class="cell-title"><a href="https://www.notion.so/3-2cc3e68f28454bc29fd3f2e5f929e819">3</a></td><td class="cell-E{uN">4개 번호가 일치</td></tr><tr id="1e6df9ab-7621-4e6f-a5e1-e31939062040"><td class="cell-title"><a href="https://www.notion.so/4-1e6df9ab76214e6fa5e1e31939062040">4</a></td><td class="cell-E{uN">3개 번호가 일치</td></tr><tr id="b613e392-9c23-4769-b1d9-6dc69007fc43"><td class="cell-title"><a href="https://www.notion.so/5-b613e3929c234769b1d96dc69007fc43">5</a></td><td class="cell-E{uN">2개 번호가 일치</td></tr><tr id="699d874e-186b-4c22-9edf-b59ead352a09"><td class="cell-title"><a href="https://www.notion.so/6-699d874e186b4c229edfb59ead352a09">6(낙첨)</a></td><td class="cell-E{uN">그 외</td></tr></tbody></table></div>

로또를 구매한 민우는 당첨 번호 발표일을 학수고대하고 있었습니다. 하지만, 민우의 동생이 로또에 낙서를 하여, 일부 번호를 알아볼 수 없게 되었습니다. 당첨 번호 발표 후, 민우는 자신이 구매했던 로또로 당첨이 가능했던 최고 순위와 최저 순위를 알아보고 싶어 졌습니다.알아볼 수 없는 번호를 0으로 표기하기로 하고, 민우가 구매한 로또 번호 6개가 44, 1, 0, 0, 31 25라고 가정해보겠습니다. 당첨 번호 6개가 31, 10, 45, 1, 6, 19라면, 당첨 가능한 최고 순위와 최저 순위의 한 예는 아래와 같습니다.
<div class="page-body"><table class="collection-content"><thead><tr><th>당첨 번호</th><th>31</th><th>10</th><th>45</th><th>1</th><th>6</th><th>19</th><th>결과</th></tr></thead><tbody><tr id="fc8942ca-27fe-4d48-b076-2b466c49a5cc"><td class="cell-title"><a href="https://www.notion.so/fc8942ca27fe4d48b0762b466c49a5cc">최고 순위 번호</a></td><td class="cell-ysUb">31</td><td class="cell-Qh`M">0 → 10</td><td class="cell-L:qt">44</td><td class="cell-i}qG">1</td><td class="cell-t`~a">0 → 6</td><td class="cell-`Lfg">25</td><td class="cell-Uwhj">4개 번호 일치, 3등</td></tr><tr id="f17df000-c9c9-49fe-ae7e-16bd88b41059"><td class="cell-title"><a href="https://www.notion.so/f17df000c9c949feae7e16bd88b41059">최저 순위 번호</a></td><td class="cell-ysUb">31</td><td class="cell-Qh`M">0 → 11</td><td class="cell-L:qt">44</td><td class="cell-i}qG">1</td><td class="cell-t`~a">0 → 7</td><td class="cell-`Lfg">25</td><td class="cell-Uwhj">2개 번호 일치, 5등</td></tr></tbody></table></div>

- 순서와 상관없이, 구매한 로또에 당첨 번호와 일치하는 번호가 있으면 맞힌 걸로 인정됩니다.
- 알아볼 수 없는 두 개의 번호를 각각 10, 6이라고 가정하면 3등에 당첨될 수 있습니다.
    - 3등을 만드는 다른 방법들도 존재합니다. 하지만, 2등 이상으로 만드는 것은 불가능합니다.
- 알아볼 수 없는 두 개의 번호를 각각 11, 7이라고 가정하면 5등에 당첨될 수 있습니다.
    - 5등을 만드는 다른 방법들도 존재합니다. 하지만, 6등(낙첨)으로 만드는 것은 불가능합니다.

민우가 구매한 로또 번호를 담은 배열 lottos, 당첨 번호를 담은 배열 win_nums가 매개변수로 주어집니다. 이때, 당첨 가능한 최고 순위와 최저 순위를 차례대로 배열에 담아서 return 하도록 solution 함수를 완성해주세요.

### 제한사항

- lottos는 길이 6인 정수 배열입니다.
- lottos의 모든 원소는 0 이상 45 이하인 정수입니다.
    - 0은 알아볼 수 없는 숫자를 의미합니다.
    - 0을 제외한 다른 숫자들은 lottos에 2개 이상 담겨있지 않습니다.
    - lottos의 원소들은 정렬되어 있지 않을 수도 있습니다.
- win_nums은 길이 6인 정수 배열입니다.
- win_nums의 모든 원소는 1 이상 45 이하인 정수입니다.
    - win_nums에는 같은 숫자가 2개 이상 담겨있지 않습니다.
    - win_nums의 원소들은 정렬되어 있지 않을 수도 있습니다.

---

### 입출력 예

<div class="page-body"><table class="collection-content"><thead><tr><th>lottos</th><th>win_nums</th><th>result</th></tr></thead><tbody><tr id="e0bfe6dc-a8ed-40d3-9d75-4145f52d22bf"><td class="cell-title"><a href="https://www.notion.so/44-1-0-0-31-25-e0bfe6dca8ed40d39d754145f52d22bf">[44, 1, 0, 0, 31, 25]</a></td><td class="cell-:JcV">[31, 10, 45, 1, 6, 19]</td><td class="cell-b[eY">[3, 5]</td></tr><tr id="05f1129e-52ac-4b96-8c3f-5a40ce17fb4e"><td class="cell-title"><a href="https://www.notion.so/0-0-0-0-0-0-05f1129e52ac4b968c3f5a40ce17fb4e">[0, 0, 0, 0, 0, 0]</a></td><td class="cell-:JcV">[38, 19, 20, 40, 15, 25]</td><td class="cell-b[eY">[1, 6]</td></tr><tr id="a8a5d719-32a7-43b2-9cb0-9bcb703bb5b1"><td class="cell-title"><a href="https://www.notion.so/45-4-35-20-3-9-a8a5d71932a743b29cb09bcb703bb5b1">[45, 4, 35, 20, 3, 9]</a></td><td class="cell-:JcV">[20, 9, 3, 45, 4, 35]</td><td class="cell-b[eY">[1, 1]</td></tr></tbody></table></div>

### 입출력 예 설명

입출력 예 #1문제 예시와 같습니다.

입출력 예 #2알아볼 수 없는 번호들이 아래와 같았다면, 1등과 6등에 당첨될 수 있습니다.
<div class="page-body"><table class="collection-content"><thead><tr><th>당첨 번호</th><th>38</th><th>19</th><th>20</th><th>40</th><th>15</th><th>25</th><th>결과</th></tr></thead><tbody><tr id="8398a579-d56c-4c96-81c1-04517e3473de"><td class="cell-title"><a href="https://www.notion.so/8398a579d56c4c9681c104517e3473de">최고 순위 번호</a></td><td class="cell-k:<O">0 → 38</td><td class="cell-;O~X">0 → 19</td><td class="cell-D;IP">0 → 20</td><td class="cell-YW=^">0 → 40</td><td class="cell-phna">0 → 15</td><td class="cell-ZVwU">0 → 25</td><td class="cell-v^qm">6개 번호 일치, 1등</td></tr><tr id="a4da77a8-b198-488a-9e72-e5e71ae8a136"><td class="cell-title"><a href="https://www.notion.so/a4da77a8b198488a9e72e5e71ae8a136">최저 순위 번호</a></td><td class="cell-k:<O">0 → 21</td><td class="cell-;O~X">0 → 22</td><td class="cell-D;IP">0 → 23</td><td class="cell-YW=^">0 → 24</td><td class="cell-phna">0 → 26</td><td class="cell-ZVwU">0 → 27</td><td class="cell-v^qm">0개 번호 일치, 6등</td></tr></tbody></table></div>

입출력 예 #3민우가 구매한 로또의 번호와 당첨 번호가 모두 일치하므로, 최고 순위와 최저 순위는 모두 1등입니다.

# 풀이

```python
def solution(lottos, win_nums):
    rank_table = {
        0 : 6,
        1 : 6,
        2 : 5,
        3 : 4,
        4 : 3,
        5 : 2,
        6 : 1
    }

    zeros = lottos.count(0)
    correct = 0

    for i in lottos:
        if i in win_nums:
            correct += 1

    return [rank_table[correct+zeros], rank_table[correct]]
```

먼저 `key`값으로 로또 번호를 맞춘 개수로 하는 순위 테이블을 하나 만들었다. `value`는 맞춘 개수에 해당하는 순위이다.

다음은 `0`의 개수를 파악하였고 그 이유는 모든 `0`이 맞는 번호일경우가 최대 순위이고 최악의 경우는 지워지지 않은 (`≠0`) 번호중 맞은 개수가 최소 순위가 될것이기 때문이다 따라서 이 개수를 먼저 파악한 후 지워 지지 않은 번호 중 맞은 개수를 파악한다면

- 최소 순위 : 지워지지 않은 번호 중 맞은 개수
- 최대 순위 : 지워지지 않은 번호 중 맞은 개수 + `0`의 개수

가 될것이다.

이제 `lottos`의 번호를 하나씩 꺼내와 해당 요소가 정답에 포함되어 있는지 확인한다. 만약 포함 된다면 지워지지 않은 번호 중 맞은 개수(`correct`)를 하나 올려준다.

반복문 수행이 끝났으면 앞서 설명한대로 최소 정답개수에 대한 순위와 최대 정답개수에 대한 순위를 `rank_table`에서 찾아 리턴한다.
