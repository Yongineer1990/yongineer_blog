---
title: Sorting
date: 2021-08-13 09:08:56
category: Algorithm
thumbnail: 'https://yongineer.duckdns.org/algorithm/algorithm.png'
draft: false
---

![https://yongineer.duckdns.org/algorithm/algorithm.png](https://yongineer.duckdns.org/algorithm/algorithm.png)

# Bubble Sort

![https://yongineer.duckdns.org/algorithm/bubble_sort.png](https://yongineer.duckdns.org/algorithm/bubble_sort.png)

- 코드로 구현하면 다음과 같다.

    ```python
    def bubblesort(A):
    	for i in range(1, len(A)):
    		for j in range(0, len(A) - 1):
    			if A[j] > a[j + 1]:
    				A[j], A[j + 1] = a[j + 1], A[j]
    ```

- $n$번의 라운드로 이루어져 있으며, 각 라운드마다 배열의 아이템을 한번씩 쭉 모두 살펴본다.
- 연달아 있는 아이템 2개가 순서가 잘못되어 있는 것을 발견하면 두 아이템을 바꾼다.
- 배열 전채를 쭉 살펴보는 것을 $n$번 하기 때문에 시간 복잡도는 항상 $O(n^2)$이다.
- 매우 비효율적인 알고리즘이며 가장 느린 정렬 알고리즘이다.

# Merge Sort

![https://yongineer.duckdns.org/algorithm/merge_sort.png](https://yongineer.duckdns.org/algorithm/merge_sort.png)

- Best case 와 Worst case 모두 $O(n log n)$인 사실상 완전한 $θ(n log n)$으로 일정한 알고리즘이다.
- 대부분의 경우 퀵 정렬 보다는 느리지만 일정한 실행 속도를 제공한다.
- Stable Sort

> **💡 위 예시의 분할 정복 과정 설명**
>
> - `[7, 3, 2, 16, 24, 4, 11, 9]`인 입력값의 절반을 분리 하여 `[7, 3, 2, 16]`과 `[24, 4, 11, 9]`로 분리
> - 다시 이 분할된 값을 절반으로 분리 `[7, 3]`, `[2, 16]`, `[24, 4]`, `[11, 9]`
> - 이런식으로 더이상 쪼갤수 없을때 까지 부할 한 후 분할이 끝나면 정렬하면서 병합한다. (`[7]`, `[3]` → `[3, 7]`)

# Quick Sort

![https://yongineer.duckdns.org/algorithm/quick_sort.png](https://yongineer.duckdns.org/algorithm/quick_sort.png)

- `pivot`을 기준으로 좌우를 나누는 특징 때문에 파티션 교환 정렬(Partition-Exchange Sort)라고도 불린다.
- Merge sort와 마찬가지로 분할 정복 알고리즘
- 피벗보다 작으면 왼쪽, 크면 오른쪽과 같은 방식으로 파티셔닝하면서 쪼개 나간다.
- 퀵 정렬은 매우 빠르지만 최악의 경우 $O(n^2)$가 된다. 만약 이미 정렬된 배열이 입력값으로 들어온다면 피벗은 계속 오른쪽에 위치하게 되므로 파티셔닝이 전혀 이뤄지지 않는다. 이때 n번의 라운드에 걸쳐 결국 전체를 비교하기 때문에 버블 정렬과 같은 최악의 성능을 보이게 된다.
- 따라서 퀵 정렬은 입력값에 따라 성능 편차가 심한 편이다.
- Unstable Sort이다.
- 퀵 정렬은 여러가지 변형과 개선 버전이 있는데 이중 N. Lomuto의 파티션 계획을 코드로 구현하면 다음과 같다.

    ```python
    def quicksort(A, lo, hi):
    		def partition(lo, hi):
    				pivot = A[hi]
    				left = lo
    				for right in range(lo, hi):
    						if A[right] < pivot:
    							A[left], A[right] = A[right], A[left]
    							left += 1
    				A[left], A[hi] = A[hi], A[left]
    				return left

    		if lo < hi:
    				pivot = partition(lo, hi)
    				quicksort(A, lo, pivot - 1)
    				quicksort(A, pivot + 1, hi)
    ```

    - 퀵 정렬의 메인 함수에서 부터 시작하여 파티션을 나누고 각각 재귀 호출한다.
    - 이때 피벗으로 맨 오른쪽을 지정한다.
    - 이를 기준으로 2개의 포인터가 이동해서 오른쪽 포인터가 읪이 피벗보다 작다면 서로 스왑하는 형태로 진행한다. 이를 그림으로 표현하면 다음과 같다.

        ![https://yongineer.duckdns.org/algorithm/partition_scheme.png](https://yongineer.duckdns.org/algorithm/partition_scheme.png)

    - `right`포인터가 이동하면서 피벗의 값이 오른쪽 값보다 더 클때 왼쪽과 오른쪽의 스왑이 발생한다.
    - 스왑 이후 왼쪽(`left`)포인터가 함께 이동한다.
    - 여기서 피벗의 값은 5이기때문에 오른쪽 포인터가 끝에 도달하면 5미만인 값은 왼쪽으로, 5 이상인 값은 오른쪽에 위치하게 된다.
    - 그리고 왼쪽 포인터의 위치로 피벗아이템이 이동한다.
    - 최종 결과를 보면 5를 기준으로 작은 값은 왼쪽에, 큰 값은 오른쪽으로 반할되어 있고 피벗이 그 중앙으로 이동하는 모습을 확인할 수 있다.
    - 이렇게 계속 분할 하면서 정복을 진행하여 코드 기준으로 `lo < hi`를 만족하지 않을때 까지, 즉 서로 위치가 역전할 때 까지 계속 재귀로 반복되면서 정렬이 완료된다.
