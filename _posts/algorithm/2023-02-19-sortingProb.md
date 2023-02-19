---
layout: post
title:  "정렬 - 문제"
author: yj
tags: [  Algorithm, 이코테 ]
category: [ Algorithm🧩 ]
---

## <a href="#">문제 1: 위에서 아래로</a>

하나의 수열에 다양한 수가 존재한다. 이러한 수는 크기에 상관없이 나열되어 있다. 

이 수를 큰 수부터 작은 수의 순서로 정렬해야 한다. 

수열을 내림차순으로 정렬하는 프로그램을 만드시오.

**입력**
- 첫째 줄: 수열에 속해있는 수의 개수 n
- 두째줄 ~ : n개의 수

**출력**
- 입력된 수열이 내림차순으로 정렬된 결과

**해결 방법**

- 파이썬의 내장 정렬 라이브러리를 통해 내림차 순으로 정렬

**코드**

```python
n = int(input())
array = []

for _ in range(n):
    array.append(int(input()))
    
array = sorted(array, reverse=True)

print(*array, end=" ")
```


## <a href="#">문제 2: 성적이 낮은 순으로 학생 출력</a>

n명의 학생과 성적이 있다. 성적이 낮은 순서대로 학생의 이름을 출력하시오.

**입력**
- 첫째 줄: 학생 수 n
- 둘째 줄 ~ : 학생이름 정보

**출력**
- 성적이 낮은 순으로 이름 출력

**해결 방법**
- 파이썬의 내장 정렬 라이브러리를 통해 정렬

**코드**

```python
n = int(input())
array = []

for _ in range(n):
    data = input().split()
    array.append((data[0], int(data[1])))

array = sorted(array, key=lambda student: student[1])


for student in array:
    print(student[0], end = " ")
```

## <a href="#">문제 3: 두 배열의 원소 교체</a>

두개의 배열 A와 B는 N개의 원소로 구성되어있고, 배열의 원소는 모두 자연수이다.

최대 K번의 바꾸기 연산을 수행하여 배열 A의 모든 원소 합이 최대가 되도록 하는 프로그램 작성하시오.

**입력**
- 첫 번째 줄: n, k
- 두 번째 줄: 배열 A의 원소
- 세 번째 줄: 배열 B의 원소

**출력**
- 배열 A의 모든 원소의 합

**해결 방법**

A에서 가장 작은 원소, B에서 가장 큰 원소를 골라 A의 원소가 더 작을 경우 A, B 원소를 교체한다.
- A는 오름차순 정렬, B는 내림차순 정렬
- A의 원소, B의 원소 비교 후 교체 or 종료

**코드**

```python
n, k = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

a.sort()
b = sorted(b, reverse=True)

for i in range(k):
    if a[i] < b[i]:
        a[i], b[i] = b[i], a[i]
    else:
        break

print(sum(a))
```