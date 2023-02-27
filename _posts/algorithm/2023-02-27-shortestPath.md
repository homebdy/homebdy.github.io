---
layout: post
title:  "최단 경로 찾기 - 다익스트라 알고리즘"
author: yj
tags: [  Algorithm, 이코테 ]
category: [ Algorithm🧩 ]
---

**최단 경로 알고리즘**

가장 짧은 경로를 찾는 알고리즘
- 한 지점에서 다른 특정 지점까지의 최단 경로를 구해야 하는 경우
- 모든 지점에서 다른 모든 지점까지의 최단 경로를 모두 구해야 하는 경우 등

## <a href="#">다익스트라 알고리즘</a>

그래프에 여러 노드가 존재할 경우, 특정 노드에서 출발해 다른 노드로 가는 각각의 최단 경로를 구해주는 알고리즘
- 음의 간선이 있을 경우 적용 불가능
- 가장 비용이 적은 노드를 선택하는 과정을 반복해 `그리디 알고리즘`으로 분류되기도 한다

**과정**

1. 출발 노드 설정
2. 최단 거리 테이블 초기화
3. 방문하지 않은 노드 중 최단 거리가 가장 짧은 노드 선택
4. 해당 노드를 거쳐 다른 노드로 가는 비용 계산하여 최단 거리 테이블 갱신
5. 3, 4번 과정 반복

최단 경로를 구하며 각 노드에 대한 현재까지의 최단 거리 정보를 리스트에 저장하고 계속 갱신한다.

**예시**

출발 노드가 1이라하고 1번 노드에서 다른 모든 노드로의 최단 거리를 계산한다.

1. 방문하지 않은 노드 중 최단 거리가 가장 짧은 노드를 선택한다.
    - 출발 노드~출발 노드의 거리는 0이므로 출발 노드가 선택 된다.

2. 1번 노드와 연결된 모든 간선을 하나씩 확인한다.
    - 2번:2, 3번: 5, 4번: 1

3. 4번 노드를 거쳐서 갈 수 있는 노드를 조사한다.
    - 3번: 1 + 3 = 4
    - 5번: 1 + 1 = 2

4. 2번 노드를 선택해 노드 조사
    - 3번: 2 + 3 = 5, 4번 노드로 연결하는 것이 비용이 더 적으므로 갱신 X

5. 모든 노드를 조사할 떄까지 같은 과정 반복

한 단계 당 하나의 노드에 대한 최단 거리를 확실하게 찾는 방식

**구현 방식**

1. 간단한 방식

각 노드에 대한 1차원 리스트를 선언하고 이후 단계마다 1차원 리스트의 모든 원소를 순차적으로 탐색한다.

```python
import sys
input = sys.stdin.readline
INF = int(1e9) 

n, m = map(int, input().split())
start = int(input())
graph = [[] for i in range(n + 1)]
visited = [False] * (n + 1)
distance = [INF] * (n + 1)

for _ in range(m):
    a, b, c = map(int, input().split())
    graph[a].append((b, c))

# 최단 거리가 짧은 노드의 번호 반환
def get_smallest_node():
    min_value = INF
    index = 0
    for i in range(1, n + 1):
        if distance[i] < min_value and not visited[i]:
            min_value = distance[i]
            index = i
    return index

def dijkstra(start):
    distance[start] = 0
    visited[start] = True
    for j in graph[start]:
        distance[j[0]] = j[1]
    for i in range(n - 1):
        now = get_smallest_node()
        visited[now] = True
        for j in graph[now]:
            cost = distance[now] + j[1]
            if cost < distance[j[0]]:
                distance[j[0]] = cost

dijkstra(start)
for i in range(1, n + 1):
    if distance[i] == INF:
        print("INFINITY")
    else:
        print(distance[i])
```

- 시간 복잡도: O(V^2)


2. 복잡하지만 빠른 방식

- 힙 자료구조 사용하는 방식

```python
import heapq
import sys
input = sys.stdin.readline
INF = int(1e9) 

n, m = map(int, input().split())
start = int(input())
graph = [[] for i in range(n + 1)]
distance = [INF] * (n + 1)

for _ in range(m):
    a, b, c = map(int, input().split())
    graph[a].append((b, c))

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

for i in range(1, n + 1):
    if distance[i] == INF:
        print("INFINITY")
    else:
        print(distance[i])
```

- 시간 복잡도: O(ElogV)
- 한 번 처리된 노드는 더 이상 처리되지 않기 때문에 1번보다 빠르다.
- E개의 원소를 우선순위 큐에 넣었다가 모두 빼내는 연산과 유사