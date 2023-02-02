2023/01/31

[**완주하지 못한 선수**](https://school.programmers.co.kr/learn/courses/30/lessons/42576)

이 문제는 해시 관련 문제이다.

해시에 대해 아직 자세히 알아본 적이 없었기 때문에, 이것저것 찾아보며 문제를 풀었던 것 같다. 

이 문제를 풀면서 많은 시도를 해봤는데, 가장 큰 문제는 동명이인이 있을 수 있다는 전제였던 것 같다.

```python
def solution(participant, completion):		
		# participant.sort()
    # completion.sort()
    
    # (1) 반복문으로 
    # for i in completion:
    #   if i not in participant 
    # -> 중복인 원소가 있을 경우 null이라 실패
     
    # (2) zip으로 묶어서 같은 원소일 때 리스트에서 삭제할 경우
    # for x, y in zip(participant, completion):
    #     print(x, y)
    #     if x == y:
    #         participant.remove(x)
    #         completion.remove(y)
    #     else:
    #         return x
    # -> zip()은 길이가 다른 리스트끼리 묶일 경우 나머지 원소를 보여주지 않아서 실패
    # -> 그러면 completion에 임의값을 넣으면 어떻게 될까? 
    
    # (3)
    # completion.append('-')
    
    # for x, y in zip(participant, completion):
    #     if x == y:
    #         participant.remove(x)
    #         completion.remove(y)
    #     else:
    #         return x
    # -> 테스트 케이스는 통과하였으나 정확성 테스트에서 4문제 오답
    # -> 효율성 테스트는 1문제를 제외한 나머지 전부 시간 초과
    
    # (4)
    # participant_dict = {i : False for i in participant}
    # participant_dict.update({i : True for i in completion})
    # for key, value in participant_dict.items():
    #     if value == False:
    #         return key
    # 소요 시간은 줄이고, 정확도는 높였으나 딕셔너리의 중복 키가 저장되지 않아 동명이인 문제를 해결하지 못함
```

해시에 대한 영상을 찾아보고 딕셔너리를 사용해봤지만, 딕셔너리의 중복 키를 허용하지 않는 문제 때문에 동명이인 문제에서 계속 틀렸다! 다시 간단하게, 1차원적으로 생각해보자 고민하고, 답을 찾아냈다.

```python
def solution(participant, completion):	
	participant.sort()
  completion.sort()
  # 정렬한 후 서로 같은지 비교, 아닐시 participant를 return, 모두 같으면 마지막에 남은 한 사람을 return
  for i in range(len(completion)):
      if participant[i] != completion[i]:
          return participant[i]
  return participant[-1]
```

정말 간단하게도, 각 리스트를 사전순으로 정렬한 후 서로를 비교하는 것이다.

참여자가 완주를 했다면 같은 순번에 같은 이름이 있겠지만, 그렇지 않으면 순서가 어긋나서 짝이 맞지 않는 경우가 생길 것이다. 그러면 이때의 참여자 `participant[i]`를 return하고, 모두 조회해봤을 때 전부 짝이 맞는다면, 마지막 남은 한 명은 자연스럽게 완주를 하지 못한 사람이 되므로 `participant[-1]`을 return한다!

[**랜선 자르기**](https://www.acmicpc.net/problem/1654)

→ 이진탐색 / [이진탐색 알고리즘](https://wayhome25.github.io/cs/2017/04/15/cs-16/)

처음 백준의 문제를 풀어보는 것이나 다름 없어서 문제의 형식부터 이해가 어려웠다. 

이 부분에 대해서는 백준에서 연습 문제를 많이 풀어봐야 할 것 같다…… ㅠ.ㅠ 

백준 연습 : [https://www.acmicpc.net/step](https://www.acmicpc.net/step) (단계별) 

문제의 이해가 어려워서 서치를 통해 [다른 사람의 풀이](https://duckracoon.tistory.com/entry/%EB%B0%B1%EC%A4%80-1654-%EB%9E%9C%EC%84%A0%EC%9E%90%EB%A5%B4%EA%B8%B0-%ED%95%B4%EC%84%A4-python)를 보고 이해를 했다.

위의 해설을 보지 않고 문제를 풀려고 할 때는 정말!! 이해가 가지 않았는데,

해설을 보니까 알고리즘을 어떻게 풀어야 하는지 그 순서도 이해가 갔고,

문제와 풀이까지 이해가 된 것 같아서 안심했다.

우선 리스트의 중간값을 기준으로 두 개의 리스트로 분할한 후,

찾는 값이 중간값보다 크면 중간값 앞의 리스트로, 

찾는 값이 중간값보다 작으면 중간값 뒤의 리스트로 가서 검색을 한다.

그리고 찾는 값이 중간값과 같아지면 그 값을 리턴하는 것이 이진 탐색의 기본 원리였던 것이다!

해설의 코드는 아래와 같다.

```python
k,n= map(int, input().split())
line = []
for _ in range(k):
    line.append(int(input()))
start = 1
end = max(line)
 
while(start<=end):
    mid = (start+end)//2
    cnt=sum([x//mid for x in line])

    if cnt>=n:
      start=mid+1 # 자른개수가 많으면 더 크게 잘라야함
    else:
      end=mid-1

print(end)
```
