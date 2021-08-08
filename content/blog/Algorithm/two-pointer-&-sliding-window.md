---
title: Two Pointer & Sliding Window
date: 2021-08-08 17:08:72
category: Algorithm
thumbnail: 'https://yongineer.duckdns.org/algorithm/algorithm.png'
draft: false
---

![https://yongineer.duckdns.org/algorithm/algorithm.png](https://yongineer.duckdns.org/algorithm/algorithm.png)

# Two Pointer

![https://yongineer.duckdns.org/algorithm/two_pointer.png](https://yongineer.duckdns.org/algorithm/two_pointer.png)

- 시작점과 끝점 또는 왼쪽 포인터와 오른쪽 포인터 두 지점을 기준으로 하는 문제 풀이 전략
- 범위를 좁혀 나가기 위해서는 일반적으로 배열이 정렬되어 있을 때 좀 더 유리하다.

> **⚠️ 위 예시의 Two pointer 전략 설명**
> - 시작점은 첫번째 원소인 인덱스(0)을 가리키고 끝점은 마지막 원소인 인덱스(5)를 가리킨다.
> - 현재 위치의 합이 Target(8)보다 크다면 끝점의 위치를 감소 시킨다.
> - 현재 위치의 합이 Target(8)보다 작다면 시작점의 위치를 증가 시킨다.
> - 현재의 위치의 합이 Target과 같다면 해당 위치를 저장하고 시작점과 끝점을 모두 증가 시킨다.
> - 만약 시작점이 끝점보다 크다면 loop 종료 (`while left > right:`)

# Sliding Window

![https://yongineer.duckdns.org/algorithm/sliding_window.png](https://yongineer.duckdns.org/algorithm/sliding_window.png)

- 일정한 범위를 이를 이동 시켜 값을 비교하는 문제 풀이 전략
- 이때 범위는 고정적이며 이동하는 방향은 좌 또는 우 한쪽방향으로만 이동한다.
- `Two Pointer`와 달리 정렬되지 않은 배열에서도 사용 가능하다.

> **⚠️ 위 예시의 Silding Window 전략 설명**
> - `k`는 주어진 구간이다. 주어진 구간 내 수를 합하여 최대 값을 찾는 문제이다.
> - 시작점(`i`)으로 부터 `k`까지 수들의 합을 구한다. → 1️⃣
> - 이후, `i`와 `k+i`한 범위의 수들의 합을 구한 뒤 1️⃣ 과 비교하여 최대값을 저장한다.
> - 이 과정을 반복하여 최대값을 구한다.
