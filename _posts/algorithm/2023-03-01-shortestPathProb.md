---
layout: post
title:  "최단 경로 찾기 -문제"
author: yj
tags: [  Algorithm, 이코테 ]
category: [ Algorithm🧩 ]
---

## <a href="#">문제 1: 미래 도시</a>

방문 판매원 A는 많은 회사가 모여 있는 공중 미래도시에 있다. 공중 미래 도시에는 1번부터 N번까지의 회사가 있으며 특정 회사끼리는 서로 도로로 연결되어있다.

방문 판매원 A는 현재 1번 회사에 위치해 있으며, X번 회사에 망문해 물건을 팔고자 한다.

공중 미래 도시에서 특정 회사에 도착하기 위한 방법은 연결된 도로를 통해 가는 방법밖에 없으며 1초의 이동시간이 소요된다.

방문 판매원 A는 K번 회사에서 소개팅을 한 후 X 회사로 가 물건을 판매할 예정이다.

방문 판매원 A의 최소 시간으로 이동할 방법을 계산하시오.

**입력**
- 첫째 줄: 전체 회사 갯수 n, 경로 개수 M
- 두째 줄 ~ : 연결된 두 회사의 번호
- 마지막 줄: X, K 

**출력**
- 최소 이동 시간
- X번 회사로 이동할 수 없는 경우 -1 출력

**해결 방법**

- 플로이드 워셜 알고리즘 사용

**코드**

```python
INF = int(1e9)

n, m = map(int, input().split())
graph = [[INF] * (n + 1) for _ in range(n + 1)]

for a in range(1, n + 1):
    for b in range(1, n + 1):
        if a == b:
            graph[a][b] = 0

for _ in range(m):
    a, b = map(int, input().split())
    graph[a][b] = 1
    graph[b][a] = 1

x, k = map(int, input().split())

for k in range(1, n + 1):
    for a in range(1, n + 1):
        for b in range(1, n + 1):
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])

distance = graph[1][k] + graph[k][x]

if distance >= 1e9:
    print("-1")
else:
    print(distance)
```

## <a href="#">문제 3: 전보</a>

어떤 나라의 N개의 도시가 있다. 각 도시는 보내고자 하는 메시지가 있는 경우 다른 도시로 전보를 보내 다른 도시로 해당 메시지를 전송한다.

하지만 도시 사이 통로가 설치되어 있어야 메시지를 전송할 수 있으며 통로를 거쳐 메시지를 보낼 때 일정 시간이 소요된다.

어느날 도시 C에서 위급 상황이 발생하였다.

그래서 최대한 많은 도시로 메시지를 보내고자 한다.

메시지는 도시 C에서 출발해 각 도시 사이에 설치된 통로를 거쳐 최대한 많이 퍼져나갈 것이다.

도시 C에서 보낸 메시지를 받게 되는 도시의 개수와 메시지를 받는데까지 걸리는 시간이 얼마인지 계산하는 프로그램을 작성하시오.

**입력**
- 첫째 줄: 도시 개수 n, 통로 개수 M, 보내려는 도시 C
- 두째 줄 ~ : 통로에 정보 a, b와 소요 시간 c

**출력**
- 보낸 메시지를 받는 도시의 총 개수와 총 걸리는 시간 출력

**해결 방법**

- 한 도시에서 다른 도시까지의 최단 거리 문제
- 다익스트라 알고리즘

**코드**

```python
import heapq
import sys
input = sys.stdin.readline
INF = int(1e9) 
n, m, start = map(int, input().split())
graph = [[] for i in range(n + 1)]
distance = [INF] * (n + 1)

for _ in range(m):
    x, y, z = map(int, input().split())
    graph[x].append((y, z))

def dijkstra(start):
   q = []
   heapq.heappush(q, (0, start))
   distance[start] = 0
   while q:
        dist, now = heapq.heappop(q)
        if distance[now] < dist:
            continue
        for i in graph[now]:
            cost = dist + i[1]
            if cost < distance[i[0]]:
                distance[i[0]] = cost
                heapq.heappush(q, (cost, i[0]))

dijkstra(start)

count = 0
max_distance = 0
for d in distance:
    if d != 1e9:
        count += 1
        max_distance = max(max_distance, d)

print(count - 1, max_distance)
```