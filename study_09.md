> 2023년 2월 2주

이번에 공부할 알고리즘은 **백트래킹**

- **[백트래킹 (DFS와의 차이)](https://veggie-garden.tistory.com/24)**
- [**백트래킹 기법의 이해**](https://www.fun-coding.org/Chapter21-backtracking-live.html#gsc.tab=0)
- [**실전 알고리즘 - 백트래킹](https://blog.encrypted.gg/945)** (c++)

**백트래킹** : 규칙을 적용하여 얻은 결과가 틀리면 그 규칙을 적용한 다음부터 현재까지의 결과를 무시하고 처음으로 돌아가서 다른 규칙을 선택하여 다시 시도한다. (네이버 사전)

여기서 백트래킹과 DFS의 차이는, DFS는 모든 경우의 수를 탐색하는 대신, **백트래킹은 불필요한 탐색을 하지 않는다**는 점이다.

대부분의 설명글에서 백준의 [N과 M(1)](https://www.acmicpc.net/problem/15649) 문제를 예로 드는데, 문제의 풀이 코드는 아래와 같다.

재귀함수를 어떻게 잘 사용하는가가 관건인 것 같다!

```python
N, M = map(int, input().split())
ans = []

def back():
    if len(ans) == M: # 배열의 길이를 확인
        print(" ".join(map(str, ans))) # 1 2 3 이런 상태로 출력하기 위해
        return 
    for i in range(1, N+1): # 1 ~ N 까지
        if i not in ans: # 중복 확인
            ans.append(i) # 배열 추가
            back() # 재귀
            ans.pop() # return으로 돌아오면 이게 실행됨. 1, 2, 3 일때 3을 없앰으로 전 단계로 돌아가는 것
            
back()
```

---

### 2023/**02/09**

이제부터는 저번 스터디때 정한 것처럼, 일정 시간을 넘기면 더 이상 시간을 끌지 않고 풀이와 함께 코드를 이해하는 식으로 공부하기로 한다. 

그리고 요즈음 깨달은 것이라면, 당연한 소리겠지만… 아는 것도 중요하지만, 모르는 것을 아는 것도 중요하다는 점이다! 그래서 문제를 시간 내 풀지 못했을 경우 내가 어떤 부분을 몰라서 문제가 발생했는지도 꼭 적어두어야겠다. 

## **14534** **[String Permutation](https://www.acmicpc.net/problem/14534)**

### 문제

(문자열 순열) 문자열의 모든 순열을 인쇄하는 재귀적 방법을 작성합니다. 사용자는 문자 집합으로 구성된 문자열을 입력해야 합니다.

### 입력

입력의 첫 번째 줄에는 테스트 사례의 수인 T(1 ≤ T ≤ 200)가 포함됩니다. 각 테스트 사례에 대해 L(1 ≤ L ≤ 5)의 문자열이 있습니다.

### 출력

각 테스트 사례에 대해 "사례 번호 x:" 형식의 행을 출력합니다. 여기서 x는 사례 번호(1로 시작)이고 문자열 순열 집합이 이어집니다.

입력 예제는 아래와 같다.

‘abc’, ‘zxyw’, ‘p7*’ 총 3개의 case가 있다는 뜻으로 가장 첫번째 줄에 3이 써있는 것 같다.

```python
3
abc
zxyw
p7*
```

case 1인 abc의 경우, 출력은 아래와 같이 나와야 한다.

```
Case # 1:
abc
acb
bac
bca
cab
cba
```

abc → 3자리수만큼의 케이스 추가

zxyw → 4자리수만큼의 케이스 추가

p7* → 3자리수만큼의 케이스 추가

이런식으로 주어진 문자열의 길이만큼 나올 수 있는 여러가지 경우의 수를 구해내는 게 아닐까?

… 여기까지 생각했지만, 아직은 다른 코드를 보고 응용할 만큼의 능력치가 쌓이지 않은 것 같다.

N과 M(1) 문제의 구조와 아주 유사하다는 것까지는 이해할 수 있었다.

1. 가장 첫 줄에 나오는 숫자를 M이라고 생각하자 (예제: 3)
2. 빈 배열을 만들고 원소들을 조합해나가기 (append 사용)
3. **재귀? … ← 여기서부터 생각을 할 수 없게됨**

### 문제점

- 재귀를 언제 어디에 적절하게 사용해야 하는가
- 입력을 어떻게 넣어야 좋은가
    
    → [백준 문제 입력 형식 정리 (파이썬)](https://daebaq27.tistory.com/57), [파이썬 한번에 여러개 입력받기](https://happyeuni.tistory.com/18)
    

**<풀이 코드>**

[참고 풀이 링크](https://github.com/juwkim/boj/blob/3a2e1b23f80d160f94f364c681e6e24d80c484e4/%EB%B0%B1%EC%A4%80/Silver/14534.%E2%80%85String%E2%80%85Permutation/String%E2%80%85Permutation.py)

```python
from itertools import permutations

for _ in range(1, 1 + int(input())):
    print(f'Case # {_}:')
    s = input()
    for __ in permutations(s, len(s)):
        print(''.join(__))
```

이 문제의 풀이를 구글 서치로는 찾을 수가 없어서 깃허브에서 검색해봤는데, 그나마 발견한 코드는 백트래킹 설명에서 봤던 것과는 조금 다른 방식의 풀이인 것 같았다.

그래서 첫번째 줄부터 차근차근 무엇인지 찾아보기로 했다.

`from itertools import permutations` → [파이썬 순열과 조합](https://pearlluck.tistory.com/468), [Itertools/Collections 모듈](https://velog.io/@pengu/%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%9D%84-%ED%8C%8C%EC%9D%B4%EC%8D%AC%EB%8B%B5%EA%B2%8C-Itertools-Collections-%EB%AA%A8%EB%93%88)

**itertools**는 **순열과 조합을 손쉽게 만들어주는 파이썬의 모듈**이다.

우선 range로 입력된 만큼을 반복한다.

그런데 `int(input())` 이 부분이 뭔지 궁금해졌다. 

입력예제는 대부분 문자로 이루어져 있는데, int를 쓰면 어떻게 값이 반환되는 걸까?

가장 첫번째 줄의 케이스 개수인 **3**이 들어가는 걸까?

 `print(f'Case # {_}:')` 은 답을 출력하기 위한 코드인 것 같고, 중요한 부분은 이 부분인 것 같다.

```python
s = input()
    for __ in permutations(s, len(s)):
        print(''.join(__))
```

permutations의 리턴값은 객체가 되며, 경우의 수에 대한 쌍을 튜플 형식으로 반환한다고 한다.

**순서를 고려하여, 중복없이 뽑을 경우의 수**를 다룬다.

형식은 `permutations(객체, r)` 로

- 객체 : 반복 가능한 객체(리스트, 튜플, 문자열…)
- **반복 가능한 객체 안에서 r개를 선택**한다.

여기서 s가 abc일 때, abc에서 의 길이인 3만큼을 선택하는 것이다.

그러면 abc에서 중복없이 3가지를 뽑은 모든 경우의 수를 itertools의 permutations가 계산해주는 것이다..

permutations는 그냥 print하면 각 쌍들을 튜플 형식으로 반환하기 때문에 `print(''.join(__))` 으로 그 부분을 해결한 것 같다!

추가로 반복문에 들어있는 `__` 가 궁금해서 검색해봤더니,  [파이썬의 언더스코어](https://mingrammer.com/underscore-in-python/) 언더바의 역할에 대해 정리된 글을 보게 되었다. 다만, 언더바 1개가 반복문에 들어간 경우는 설명되어있지만 2개가 들어간 경우를 설명한 글은 찾기 어려웠다. 다른 차이가 있는 걸까? 🙄

## **10597 [순열장난](https://www.acmicpc.net/problem/10597)**

### 문제

kriii는 **1부터 N까지의 수로 이루어진 순열**을 파일로 저장해 놓았다. 모든 수는 10진수로 이루어져 있고, 모두 공백으로 분리되어 있다. 그런데 sujin이 그 파일의 모든 공백을 지워버렸다! kriii가 순열을 복구하도록 도와주자.

### 입력

첫 줄에 공백이 사라진 kriii의 수열이 주어진다.

kriii의 순열은 **최소 1개 최대 50개의 수**로 이루어져 있다.

### 출력

복구된 수열을 출력한다. 공백을 잊으면 안 된다.

**복구한 수열의 경우가 여러 가지 일 경우, 그 중 하나를 출력**한다.

```python
# 예제 입력1
4111109876532

# 예제 출력1
4 1 11 10 9 8 7 6 5 3 2
```

순서대로 하나씩 떼어놓되, 앞에 중복된 숫자가 있으면 한칸 건너 뛰어서 공백을 삽입할 수는 없는걸까?

**첫번째 시도 :** 10이 안나와서 실패

```python
input1 = '4111109876532'
ans = []

for i in input1:
  if i not in ans:
    ans.append(i)
  else:
    ans[-1] += i
print(' '.join(ans))
# 4 11 11 0 9 8 7 6 5 3 2
```

**두번째 시도** : 중복이 걸러지지 않음, 10이 나오지 않음

```python
input1 = '4111109876532'
ans = []

for i in input1:
  ans.append(i)
  if ans[-2:-1] == ans[-1:]: # [-2]와 [-1]이 같을 경우
    ans[-1] += i
print(' '.join(ans))
# 4 1 11 1 11 0 9 8 7 6 5 3 2
```

**세번째 시도** : 중복을 너무 잘 걸러서(?) 실패

```python
for i in input1:
  if i not in ans:
    ans.append(i)
  if ans[-2:-1] == ans[-1:]: # [-2]와 [-1]이 같을 경우
    ans[-1] += i
print(' '.join(ans))
# 4 1 0 9 8 7 6 5 3 2
```

괜히 백트래킹 문제로 분류된 게 아닌 것 같다. 

단순히 for 문만 사용해서 해결할 게 아니라, 재귀를 사용해야 풀리는 것 같다…

위의 시도를 통해서 알아낸 문제점은, 10을 만들어야 하는 것과, 중복을 제거하는 문제인 것 같다.

**<풀이 코드>**

[문제 풀이1](https://dogsavestheworld.tistory.com/236), [문제 풀이2](https://kimmeh1.tistory.com/351), [문제 풀이3](https://velog.io/@wisepine/BOJ-%EC%88%9C%EC%97%B4-%EC%9E%A5%EB%82%9C-no.10597)

```python
import sys
sys.stdin = open('10957.txt', 'r')

def dfs(index, arr):
    if index == len(kriii):
        print(*arr)
        exit()

    # 1자리수 check
    num1 = int(kriii[index])
    if not visited[num1]:
        visited[num1] = True
        arr.append(num1)
        dfs(index + 1, arr)
        visited[num1] = False
        arr.pop()

    # 2자리수 check
    if index+1 < len(kriii):
        num2 = int(kriii[index:index+2])
        if num2 <= N and not visited[num2]:
            visited[num2] = True
            arr.append(num2)
            dfs(index+2, arr)
            visited[num2] = False
            arr.pop()

kriii = input()
N = len(kriii) if len(kriii) < 10 else 9 + (len(kriii) - 9) // 2
visited = [0 for _ in range(N + 1)]

dfs(0, [])
```

이제부터 이 코드를 차근차근 뜯어보고 이해해야한다…!

***코드 이해하기***

```python
kriii = input()
N = len(kriii) if len(kriii) < 10 else 9 + (len(kriii) - 9) // 2
visited = [0 for _ in range(N + 1)]
```

우선 함수 구성 부분은 뒤로 하고 이 부분부터 이해하려고 한다.

1부터 N까지 구성된 수열의 **N**을 찾기 위해 주어진 문자열을 잘 살펴보아야 한다.

문자열의 길이가 **10보다 작은 경우** `N = len(kriii)` 이고, 

10보다 클 경우 `N = 9 + (len(kriii) - 9) // 2` 이다.

→ `N = len(kriii) if len(kriii) < 10 else 9 + (len(kriii) - 9) // 2`

문자열이 **‘123456789’** 라면 N = **9**

**‘12345678910111213’** 라면 N = (17 - 9) // 2 + 9 = **13** 인 것이다.

여기서 **2로 나누는 이유는, 10의 자리부터는 두자리이기 때문**이다!

그리고 `visited = [0 for _ in range(N + 1)]` 로 방문 리스트를 만들어서 방문 처리를 하는 것 같다.

방문 리스트는 N의 길이만큼 0을 채워서 생성한다.

생각해보니 백트래킹 자체가 가지치기를 하는 DFS 란 비유를 봤던 것 같아서 뒤늦게 깨달았다…

```python
def dfs(index, arr):
    if index == len(kriii):
        print(*arr)
        exit()

    # 1자리수 check
    num1 = int(kriii[index])
    if not visited[num1]:
        visited[num1] = True
        arr.append(num1)
        dfs(index + 1, arr)
        visited[num1] = False
        arr.pop()

    # 2자리수 check
    if index+1 < len(kriii):
        num2 = int(kriii[index:index+2])
        if num2 <= N and not visited[num2]:
            visited[num2] = True
            arr.append(num2)
            dfs(index+2, arr)
            visited[num2] = False
            arr.pop()
```

이 풀이는 dfs 라는 함수를 만들었는데, 

0부터 시작할 인덱스와 아무것도 들어있지 않은 빈 배열을 넣고 시작한다.

우선 한자리수를 먼저 체크한다. 맨처음부터 시작한다고 가정했을 때, 인덱스는 0이다.

1. 주어진 문자열의 0번째 인덱스부터 확인하게 된다. (= `num1`)
2. `visited[num1]`이 False일 때, 방문리스트의 `num1` 자리를 True 처리하고, 배열에 `num1`을 추가.
3. dfs 함수를 다시 불러와 현재 인덱스에 1을 더한다.
4. 방문 처리를 취소하고, 배열에 추가한 값도 뺀다.

→ 여기서 4번이 처음에 이해가 되지 않았다. 방문처리를 했는데 왜 다시 취소를 하지? 했는데, 백트래킹 원리를 배울 때 **전 단계로 돌아가려면 pop을 사용해서 돌아간다**는 걸 떠올리고 이해했다.

다음으로 두자리수도 체크해야 한다.

`if index+1 < len(kriii)` ← 아직 이 부분을 이해하지 못했다 왜 index+1 이 문자열의 길이보다 작을 때 두자리수를 체크하는 걸까? 🙄

`num2`는 문자열에서 두글자를 가져온 두자리수로 한다.

그리고 그 다음부터는 한자리수를 체크할 때와 같은 과정을 거치는데, `num2`가 N보다 작거나 같을 때의 조건이 추가되고, dfs 함수를 다시 불러올 때 index + 2를 하는 것 외의 차이점은 없다. 

이렇게 해서 가장 첫줄의 if문으로 인덱스와 배열의 길이가 같으면 배열을 출력하고 종료하는 것이다.
