---
layout: post
title:  "구현 문제"
author: yj
tags: [  Algorithm, 이코테 ]
category: [ Algorithm🧩 ]
---

### <a href="#">1. 왕실의 나이트</a>

한 왕실의 정원은 체스판 같은 8*8 좌표 평면이다.

왕실 정원의 특정한 한 칸에 나이트가 서있고, 이 나이트는 매우 충성스러운 신하로 매일 무술을 연마한다.

나이트는 이동 시 L자 형태로만 이동할 수 있고, 정원 밖으로 나갈 수 없다.

나이트는 다음과 같은 2가지 형태로만 움직일 수 있다.
1. 수평으로 두 칸 이동 후 수직으로 한 칸 이동
2. 수직으로 두 칸 이동 후 수평으로 한 칸 이동

이처럼 나이트의 위치가 주어져있을 때 나이트가 이동할 수 있는 경우의 수를 출력하는 프로그램을 작성하시오.

왕실 정원의 위치는 행 위치는 1~8, 열 위치는 a~h로 표현한다.

**해결 방법**

나이트가 움직일 수 있는 경로로 모두 움직여 본다.

**코드**
```python
position = input()
row = int(position[1])
col = int(ord(position[0])) - int(ord('a')) + 1

steps = [(-2, 1), (-1, -2), (1, -2), (2, -1), (2, 1), (1, 2), (-1, 2), (-2, 1)]

result = 0

for step in steps:
    next_row = row + step[0]
    next_col = col + step[1]
    if next_col >= 1 and next_col <= 8 and next_row >= 1 and next_row <= 8:
        result += 1
    
print(result)
```
### <a href="#">2. 게임 개발</a>

현민이는 게임 캐릭터가 맵 안에서 움직이는 시스템을 개발 중이다. 

캐릭터가 있는 장소는 1\*1 크기의 정사각형으로 이루어진 n\*m 크기의 직사각형으로 각 칸은 육지 또는 바다로 나뉘어져 있다.

캐릭터는 동서남북 중 한 곳을 바라본다.

앱의 각 칸은 (A, B)로 나타내고 A는 북쪽으로부터 떨어진 칸의 개수, B는 서쪽으로부터 떨어진 칸의 개수이다.

캐릭터는 상하좌우로 움직이고 배다로 되어 있는 공간엔 갈 수 없다.

게임의 메뉴얼은 다음과 같다
1. 현재 위치에서 현재 방향을 기준으로 왼쪽 방향부터 차례대로 갈 곳을 정한다.
2. 캐릭터의 바로 왼쪽 방향에 가보지 않은 칸이 없다면 왼쪽 방향으로 회전만 수행하고 1단계로 돌아간다.
3. 만약 네 방향 모두 이미 가본 칸이거나 바다로 된 칸인 경우에는 바라보는 방향을 유지한 채 한 칸 뒤로 가고 1단계로 돌아간다. 단 뒤쪽 방향이 바다인 칸인 경우 움직임을 멈춘다.

**입력 조건**
- 첫째 줄: N, M
- 둘째 줄: 게임 캐릭터가 있는 칸의 좌표와 바라보고 있는 방향 (0:북, 1:동, 2:남, 3:서)
- 셋째 줄 ~: 맵의 정보 (육지: 0, 바다: 1)

**해결 방법**

시뮬레이션 문제로 문제에서 요구하는 내용을 오류없이 성실하게 구현해야한다.

**코드**
```python
n, m = map(int, input().split())
x, y, direction = map(int, input().split())

d = [[0] * m for _ in range(n)]
d[x][y] = 1

gameMap = []

for i in range(n):
    gameMap.append(list(map(int, input().split())))

dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

def turn_left():
    global direction
    direction -= 1
    if direction == -1:
        direction =3

count = 1
turn_time = 0

while True:
    turn_left()
    nx = x + dx[direction]
    ny = y + dy[direction]
    if d[nx][ny] == 0 and gameMap[nx][ny] == 0:
        d[nx][ny] = 1
        x = nx
        y = ny
        count += 1
        turn_time = 0
        continue
    else:
        turn_time += 1
    
    if turn_time == 4:
        nx = x - dx[direction]
        ny = y - dy[direction]
        if gameMap[nx][ny] == 0:
            x = nx
            y = ny
        else:
            break
        turn_time = 0

print(count)
```