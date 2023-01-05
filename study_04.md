# 1월 1주

**01/03**

**[최대공약수와 최소공배수](https://school.programmers.co.kr/learn/courses/30/lessons/12940)**

이번 문제도 그냥 막연하게 풀려니 도무지 감이 안왔다.

그래서 최대공약수나 최소공배수를 구해내는 식이 있을까, 검색해보았는데

[유클리드 호제법](https://ko.wikipedia.org/wiki/%EC%9C%A0%ED%81%B4%EB%A6%AC%EB%93%9C_%ED%98%B8%EC%A0%9C%EB%B2%95)이란 것을 알게 되었다.

이 유클리드 호제법으로 n과 m의 최대공약수를 구할 수 있다.

그리고 n과 m을 곱한 후 최대공약수로 나눠주면 최소공배수를 구할 수 있다!

이때, 최대공약수를 구할 때 n과 m이 변하므로 미리 위에서 n과 m을 곱한 수를 변수로 저장했다.

```python
def solution(n, m):
    answer = []
    nm = n*m
    
    # 유클리드 호제법 
    while m != 0:
        r = n % m
        n = m
        m = r
    answer.append(n) # n : 최대공약수
    
    x = nm/n
    answer.append(x) # x : 최소공배수
    
    return answer
```

[**체육복**](https://school.programmers.co.kr/learn/courses/30/lessons/42862)

생각하는 과정에서는 제법 어려운 문제였는데,

막상 풀고 보니 결과적으로는 간단한 코드가 나왔다.

우선 set을 사용해서 lost와 reserve에서 중복된 원소는 삭제를 한다.

여기에서 정렬은 set 때문에 sort를 쓸 필요없이 알아서 정렬된다.

그리고 reserve의 앞`(i-1)`이나 뒤`(i+1)`에 lost가 있으면 lost_s에서 그 번호를 삭제한다.

최종적으로 n에서 lost_s의 길이를 뺀 수를 리턴한다.

참고로! `i-1`이 아닌 `i+1`부터 확인하려고 하면 문제가 생기기 때문에 꼭 `i-1`부터 확인을 해줘야한다. 나도 처음에는 별 생각없이 `i+1`을 먼저 확인했는데, 몇가지 케이스에서 통과를 하지 못하는 문제가 생겨서 순서를 바꿨더니 문제없이 잘 통과가 되었다.

```python
def solution(n, lost, reserve):
    # lost와 reserve의 중복 삭제 + 정렬 : set 사용
    lost_s = set(lost) - set(reserve)
    reserve_s = set(reserve) - set(lost)
    
    # reserve의 앞, 뒤에 lost가 있으면 lost에서 삭제
    for i in reserve_s:
        if i-1 in lost_s:
            lost_s.remove(i-1)
        elif i+1 in lost_s:
            lost_s.remove(i+1)
            
    # 수업을 들을 수 있는 학생의 최댓값 return 
    return n - len(lost_s)
```

**01/05**

**[크레인 인형 뽑기 게임](https://school.programmers.co.kr/learn/courses/30/lessons/64061)**

처음 풀어보는 카카오의 코딩 문제였는데, 

문제도 길고 가독성이 좋지 않아 상당히 어렵게만 느껴졌었다. 

하지만 문제를 자세히 읽어보고, 직접 손으로 써보니 이와 유사했던 문제 하나가 떠올랐다. 

프로그래머스 1단계 문제인 **햄버거 쌓기**인데, 이 문제 역시 스택 문제였다. 

스택의 위에서부터 들어온 원소와 비교하고 지워나가는 것이 이 문제와 유사함을 느꼈다.

![image](https://user-images.githubusercontent.com/106129152/210744583-fa24f0c4-e2bf-41af-8668-81fb6108fd69.png)

처음에는 배열의 구조를 잘못 이해하고 열 방향이 아니라 행 방향으로 색인을 해서 통과하지 못했다. 

또, `break`가 없으면 계속해서 세로줄의 인형을 없애버리기 때문에 `break`를 쓰지 않으면 문제 통과가 어려웠다. 

**1. move로 배열의 열 방향을 먼저 탐색**

**2. 해당 move 열의 i행에서 0이 아닌 인형을 리스트에 추가함(= 가장 위의 인형)**

**3. 집어온 인형이 원래 있던 자리는 빈칸으로 만들어줌 (0으로 바꾸기)**

**4. 리스트에 인형이 2개 이상 쌓였을 때, 뒤에서 첫번째 인형과 두번째 인형이 같은지 비교**

**→ 만약 같다면 리스트[-2:] 만큼 삭제하고, 카운트에 2를 더함**

**5. 4번이 계속 수행되는 것을 막기 위해 break 선언** 

**6. 인형이 삭제된 카운트를 리턴**

![image](https://user-images.githubusercontent.com/106129152/210744625-596dc104-5135-43ec-8d10-4efaacd34bfd.png)
