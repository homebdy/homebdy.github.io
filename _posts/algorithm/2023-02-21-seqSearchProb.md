---
layout: post
title:  "이진 탐색 - 문제"
author: yj
tags: [  Algorithm, 이코테 ]
category: [ Algorithm🧩 ]
---

## <a href="#">문제 1: 부품 찾기</a>

전자 매장에 부품이 N개 있다. 각 부품은 정수 형태의 고유한 번호가 있다.

어느날 손님이 문의한 부품 M개 종류를 모두 확인해서 견적서를 요청해 부품 M개의 종류를 확인해 견적서를 작성해야한다.

부품이 있는지 확인하는 프로그램을 작성하자!

**입력**
- 첫째 줄: 정수 n
- 두째 줄: n개의 정수
- 셋째 줄: 정수 M
- 넷째 줄: M개의 정수

**출력**
- 부품이 존재하면 yes, 없으면 no

**해결 방법**

- 이진 탐색 알고리즘으로 풀 수 있다!
- M개의 찾고자 하는 부품이 존재하는지 탐색한다.

**코드**

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

n = int(input())
array = list(map(int, input().split()))
array.sort()

m = int(input())
x = list(map(int, input().split()))

for i in x:
    result = binary_serarch(array, i, 0, n-1)
    if result == None:
        print("no", end=" ")
    else:
        print("yes", end=" ")
```


## <a href="#">문제 2: 떡볶이 떡 만들기</a>

나는 여행 가신 부모님을 대신해서 떡집 일을 하기로 했다. 

오늘은 떡볶이 떡을 만드는 날이다.

떡볶이 떡의 길이가 일정하지 않지만 한 봉지에 들어가는 떡의 총 길이는 절단기로 잘라 맞춘다.

절단기에 높이 H를 지정하면 줄지어진 떡을 한 번에 절단한다.



손님이 왔을 때 요청한 총 길이가 M일 때 적어도 M만큼의 떡을 얻기 위해 절단기에 설정할 수 있는 높이의  최댓값을 구하는 프로그램을 작성하시오!

**입력**
- 첫째 줄: 첫째줄에 떡의 개수 N과 요청한 떡의 길이 M이 주어진다.
- 두번째 줄: 떡의 개별 높이

**출력**
- 적어도 M만큼의 떡을 집에 가져가기 위해 절단기에 설정할 수 있는 높이의 최댓값

**해결 방법**
- 파라메트릭 서치: 최적화 문제를 결정문제로 바꾸어 해결하는 기법
- 현재 이 높이로 자르면 조건을 만족할 수 있는지 확인
- 범위를 좁힐 때는 이진 탐색의 원리를 이용해 절반씩 탐색 범위를 좁혀 나감.

**코드**

```python
n, m = map(int, input().split())
array = list(map(int, input().split()))
array.sort()

start = 0
end = max(array)

result = 0
while start <= end:
    total = 0
    mid = (start + end) // 2
    for x in array:
        if x > mid:
            total += x - mid
        
    if total < m:
        end = mid -1
    
    else:
        result = mid
        start = mid + 1

print(result)
```