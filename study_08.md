> 2023년 2월 2일

- [BFS 알고리즘 개념](https://heytech.tistory.com/56)
- [너비 우선 탐색이란](https://gmlwjd9405.github.io/2018/08/15/algorithm-bfs.html)
- [파이썬으로 구현하는 DFS와 BFS](https://cyc1am3n.github.io/2019/04/26/bfs_dfs_with_python.html)

**1697** [숨바꼭질](https://www.acmicpc.net/problem/1697) (BFS)

deque 라이브러리를 사용하는 것은 BFS 알고리즘 설명을 찾아보고 알게 되었다.

덱은 리스트보다 속도가 훨씬 빠르고 풀이에 용이하다고 한다.

정말 하루 종일 붙들고서 한 결과가 아래의 코드이다.

선입선출의 구조인 큐를 이용해 `popleft()`로 시작 노드를 잡고 인접 노드를 큐에 `append`한다.

이 작업을 원하는 값이 나올 때까지 반복하는 것인데, 문제는 방문한 노드의 처리를 성공하지 못한 것이다. 

예제대로라면 4가 출력되어야 하는데, 아래 코드는 22가 출력된다.

```python
from collections import deque

# n, k = map(int, input().split())
n = 5
k = 17

answer = 0
queue = deque()
queue.append(n) # deque([5])

while True:
  x = queue.popleft() # 5

  queue.append(x-1)
  queue.append(x+1)     
  queue.append(x*2)
  answer += 1
  #print(queue, 'time: ', answer)

  if k in queue:
    break
    
print(answer)
```

![image](https://user-images.githubusercontent.com/106129152/216330018-1aba56bd-2fb9-4483-9f30-c2472f9e6013.png)

*~~wow…~~*

이번에는 visited 라는 빈 리스트를 만들어서 pop된 요소는 집어넣고, 

visited에 있는 원소가 queue에 생길 시 삭제하는 코드로 짜봤는데 결과로 13이 나왔다.

시간을 계산하는 문제의 해결이 가장 중요한 것 같다.

```python
from collections import deque

# n, k = map(int, input().split())
n = 5
k = 17

answer = 0
visited = []
queue = deque()
queue.append(n) # deque([5])

while True:
  x = queue.popleft() # 5
  queue.extend([x-1, x+1, x*2])
  visited.append(x)
  for v in visited:
    if v in queue:
      queue.remove(v)
  answer += 1
  print(queue, 'time: ', answer)

  if k in queue:
    break
    
print(answer)
```

![image](https://user-images.githubusercontent.com/106129152/216330073-57fa6e72-6b31-40c1-8cc7-94479e8d8f39.png)

```python
from collections import deque

# n, k = map(int, input().split())
n = 5
k = 17

visited = [0] * 100001 
queue = deque()
queue.append(n) # deque([5])

while True:
  x = queue.popleft() # 5
  queue.extend([x-1, x+1, x*2])
  visited[x+1] = x
  print(visited[0:k+1])
  for v in visited:
    if v in queue:
      queue.remove(v)
  print('queue: ', queue, '\n')

  if k in queue:
    break
```

visited에 미리 입력 가능한 크기 만큼 0을 넣고 추가하는 방식으로 바꿔보았는데, 생각해보니까 무작정 extend로 추가 확장할 게 아니라 방문한 노드와 비교해서 방문한 노드는 추가하지 못하도록 for문을 거쳐서 넣어야 맞지 않나? 라는 생각이 들었다.

또, queue 안에 k가 있는지 확인하는 것보단 수빈이의 위치인 x가 k와 같은가를 비교해야 훨씬 정확하다는 것을 뒤늦게 깨달았다.

```python
from collections import deque

n, k = map(int, input().split())

visited = [0] * 100001 
queue = deque()
queue.append(n) # deque([5]) 시작 노드부터 입력

while True:
  x = queue.popleft() # 5
  if x == k:
    print(visited[x])
    break
  for i in (x-1, x+1, x*2):
    if 0<= i <= 100000 and not visited[i]:
      visited[i] = visited[x]+1
      queue.append(i)
```

→ [관련 풀이](https://wook-2124.tistory.com/273), [풀이 영상](https://www.youtube.com/watch?v=O8K5A9ojrVw)

**4963** [섬의 개수](https://www.acmicpc.net/problem/4963) (BFS, DFS)

이 문제는 0(바다)과 1(땅)로 이루어진 지도를 주고, 섬의 개수를 구하는 문제이다.

앞의 숨바꼭질 문제에서 너무 많은 시간을 버린 탓에 이 문제는 제대로 보지 못하고 시도조차 못해봤다. 해당 알고리즘에 대해 더 공부해보고 시도해봐야 할 것 같다. 

이 문제는 상, 하, 좌, 우 네가지 방향뿐만 아닌 대각선 방향까지 포함해 총 여덟가지 방향을 적용할 수 있어야 한다. 

우선은 관련 풀이를 참고해서 코드를 보고 이해해보았다. → [관련 풀이](https://latte-is-horse.tistory.com/305)

```python
from collections import deque

w, h = int(), int()
arr, visited = list(), list()
count = 0
d = [(-1, 0), (0, -1), (1, 0), (0, 1), (-1, -1), (1, -1), (1, 1), (-1, 1)] # ↑ ← ↓ → ←↑ ←↓ ↓→ →↑

def bfs(a, b):
    global w, h, arr, count

    if not arr[a][b] or visited[a][b]: return

    queue = deque([(a, b)])
    count += 1

    while queue:
        x, y = queue.popleft()

        for i in range(8):
            nx, ny = x + d[i][0], y + d[i][1]
            if nx in range(h) and ny in range(w) and not visited[nx][ny] and arr[nx][ny]:
                visited[nx][ny] = True
                queue.append((nx, ny))

def solution():
    global w, h, arr, count, visited

    while True:
        # init
        count = 0

        # user_input
        w, h = map(int, input().split())
        if w == h == 0: break

        visited = [[False] * w for _ in range(h)]
        arr = [list(map(int, input().split())) for _ in range(h)]

        # logic
        for x in range(h):
            for y in range(w):
                bfs(x, y)
        print(count)

solution()
```

## 이번 스터디에서 느낀 점

거의 처음으로, 본격적인 알고리즘 문제를 풀어보는 것이 처음이었고, 생각보다 많이 힘들었다!

프로그래머스 1단계의 탐욕법이나 해시 문제는 어렵긴 해도 결국에는 풀어낼 수 있었는데, 이번 문제들은 차원이 다른 느낌이었다.

문제 풀이 플랫폼이 달랐던 점도 있었겠지만, 가장 큰 요인은 알고리즘에 대한 지식 부족과, 문제 풀이 경험의 부족에서 오는 어려움이었던 것 같다.

거의 이틀가량 한 문제만 붙잡고, 이해하지 못하고, 어떤 곳에서 에러가 발생하는지 바로 캐치하지 못하는 점에서 더 스트레스를 받지 않았나 싶다!

내가 너무 부족한 걸까? 이 공부를 계속 이어가도 괜찮은 걸까? 하는 부정적인 생각들이 들었었는데, 오늘 스터디에서 다른 분들과 얘기를 나눠보니, 나만 힘들었던 것이 아니었나보다.

이번 스터디에서는 상당히 얻어가는 것이 많았다!

다시 한 번 마음을 다잡고 원동력을 얻을 수 있어서 기쁘기도 했다. 

같은 고민을 나눌 수 있는 사람들이 있다는 것은 정말 다행이기도 하고, 중요하기도 하다😀

---

***앞으로의 스터디는…***

- **매주 목요일(주 1회)**
- **알고리즘 하나를 정해서 2문제 풀이&리뷰**
- **한 문제당 시간을 정해두고, 정해진 시간 안에 풀지 못할 경우 풀이를 참고해서 공부**

가장 중요한 것은 꺾이지 않는 마음!!!!!!!!!!!!!!!! 🔥🔥🔥🔥
