---
layout: post
title:  "최단 경로 찾기 - 플로이도 워셜 알고리즘"
author: yj
tags: [  Algorithm, 이코테 ]
category: [ Algorithm🧩 ]
---

**최단 경로 알고리즘**

가장 짧은 경로를 찾는 알고리즘
- 한 지점에서 다른 특정 지점까지의 최단 경로를 구해야 하는 경우
- 모든 지점에서 다른 모든 지점까지의 최단 경로를 모두 구해야 하는 경우 등

## <a href="#">플로이드 워셜 알고리즘</a>

모든 지점에서 다른 모든 지점까지의 최단 경로를 모두 구하는 경우 사용하는 알고리즘
- 한 단계마다 거쳐 가는 노드를 기준으로 알고리즘 수행
- 매번 방문하지 않은 노드 중 최단 거리를 갖는 노드를 찾을 필요가 없는 점이 다름
- 노드가 N개일 때, 알고리즘상 N번의 단계 수행
- 단계마다 O(n^2)의 연산을 통해 경로 탐색
- 총 시간 복잡도: O(N^3)
- 2차원 배열에 최단 거리 정보를 담아 DP로 볼 수 있다!


**예시**

- 각 단계마다 특정 노드 k를 거쳐 가는 경우 확인
- 점화식: Dab = min(Dab, Dak+Dkb)

| | 1번 | 2번 | 3번 | 4번 |
|---|---|---|---|---|
| 1번 | 0 | 4 | 무한 | 6 |
| 2번 | 3 | 0 | 7 | 무한 |
| 3번 | 5 | 무한 | 0 | 4 |
| 4번 | 무한 | 무한 | 2 | 0 |

1. 1번 노드를 거쳐 가는 경우 
- D23 = min(D23, D21+D13)
- D24 = min(D24, D21+D14)

... 등 모든 노드의 최소 값을 구해 리스트를 갱신한다


**구현**

```python
import sys
input = sys.stdin.readline

INF = int(1e9)

n = int(input()) 
m = int(input()) 

graph = [[INF]*(n+1) for _ in range(n+1)]

for a in range(1, n+1):
    for b in range(1, n+1):
        if a==b:
            graph[a][b]=0

for _ in range(m):
    a,b,c = map(int, input().split())
    graph[a][b] = min(graph[a][b], c) 

for k in range(1, n+1):
    for a in range(1, n+1):
        for b in range(1, n+1):
            graph[a][b] = min(graph[a][b], graph[a][k]+graph[k][b])

for a in range(1, n+1):
    for b in range(1, n+1):
        if graph[a][b] == INF:
            print(0, end=' ')
        else:
            print(graph[a][b], end=' ')
    print()
```