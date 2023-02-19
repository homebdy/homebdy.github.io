---
layout: post
title:  "이진 탐색"
author: yj
tags: [  Algorithm, 이코테 ]
category: [ Algorithm🧩 ]
---

**순차 탐색**

리스트 안에 있는 특정한 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 차례대로 확인하는 방법
- 정렬되지 않은 리스트에서 데이터를 찾을 경우 사용한다.
- 원소를 하나씩 확인하므로 최악의 경우에도 시간 복잡도가 `O(N)`이다.

## <a href="#">이진 탐색</a>

내부의 데이터가 정렬되어 있을 경우 사용할 수 있는 알고리즘

탐색의 범위를 절반씩 좁혀가며 데이터를 탐색한다.

- 탐색할 범위의 시작점, 끝점, 중간점을 이용하여 탐색
- 찾으려는 데이터의 중간점 위치에 있는 데이터를 반복적으로 비교하여 원하는 데이터를 찾는 탐색

**과정**

0 2 4 6 8 10 12 14 16 18

찾으려는 데이터 = 4

시작점 = 0 (0), 중간점 = 4 (8), 끝점 = 9 (18)

1. 중간점의 데이터인 8 < 4: 끝점을 3번으로 옮기고 중간점을 1번으로 변경 : 0 2 4 6 | 8 10 12 14 16 18 
2. 중간점의 데이터인 1 < 4: 시작점을 2로 변경, 중간점도 2로 변경 : 0 2 | 4 6 | 8 10 12 14 16 18
3. 중간점의 데이터인 4 = 찾던 데이터: 탐색 종료

- 한 번 확인할 때마다 확인하는 원소의 개수가 절반씩 줄어든다.
- 시간복잡도: O(logN)

**구현 방식**

1. 재귀 함수로 구현하는 방식

```python
def binary_serarch(array, target, start, end):
    if start > end:
        return None
    mid = (start + end) // 2
    if array[mid] == target:
        return mid
    elif array[mid] > target:
        return binary_serarch(array, target, start, mid -1)
    else:
        return binary_serarch(array, target, mid + 1, end)

n, target = map(int, input().split())
array = list(map(int, input().split()))

result = binary_serarch(array, target, 0, n-1)
if result == None:
    print("원소가 존재하지 않습니다.")
else:
    print(result+1)
```

2. 반복문으로 구현하는 방식

```python
def binary_serarch(array, target, start, end):
    while start <= end:
        mid = (start + end) // 2
        if array[mid] == target:
            return mid
        elif array[mid] > target:
            end = mid - 1
        else:
            start = mid + 1
    return None

n, target = map(int, input().split())
array = list(map(int, input().split()))

result = binary_serarch(array, target, 0, n-1)
if result == None:
    print("원소가 존재하지 않습니다.")
else:
    print(result+1)
```

- 탬색의 범위가 2000만을 넘어가면 이진탐색으로 접근해 보는 것이 좋다!

### 트리

- 노드와 노드로 연결된 자료구조
- 부모와 자식 노드의 관계로 표현
- 트리의 최상단 노드 = `루트 노드`
- 최하단 노드 = `단말 노드`
- 트리에서 일부를 떼어내도 트리구조가 유지 된다 = `서브 트리`
- 계층적이고 정렬된 데이터를 다루기 적합하다.

큰 데이터를 정리하는 소프트웨어는 대부분 데이터를 트리 자료구조로 저장해 이진탐색과 같은 탐색 기법을 이용해 빠르게 탐색이 가능하다.

#### 이진 탐색 트리
효율적인 탐색이 가능한 자료구조
- 부모 노드가 왼쪽 자식 노드보다 작다.
- 부모 노드보다 오른쪽 자식 노드가 크다.

**과정**

- 예시

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;30

&nbsp;&nbsp;17&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;48

5&nbsp;&nbsp;23&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;37&nbsp;&nbsp;50

찾는 원소 = 37

1. 루트 노드 30 방문, 37과 비교 → 30 < 37이므로 오른쪽 노드로 이동
2. 오른쪽 자식 노드인 48 방문, 비교 → 48 > 37이므로 왼쪽 노드로 이동
3. 37 발견