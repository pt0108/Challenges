# 1월 3주

**01/17**

[**기사단원의 무기**](https://school.programmers.co.kr/learn/courses/30/lessons/136798)

```python
def solution(number, limit, power):
    result = []
    # number 수만큼 약수의 개수를 구해서 리스트에 추가
    for n in range(1, number+1):
        first = [i for i in range(1, n+1) if n%i == 0]
        result.append(len(first))
    # limit보다 큰 원소는 지우고 power로 대체
    for r in result:
        if r > limit:
            result.remove(r)
            result.append(power)
    return sum(result)
```

각 원소의 약수 개수를 구해서 리스트에 추가하고, 

limit보다 큰 원소는 power로 대체하는 코드를 짰는데, 역시나 시간 초과에 걸려버리고 말았다. 

문제의 힌트를 보니까, n 전부를 순회하는 것이 아니라 

**n의 제곱근 만큼만 반복**문을 돌리면 된다는 것을 알게 되었다!

```python
# 약수의 개수를 구하는 함수 생성
def plus_number(n, limit, power):
    cnt = 0
    for i in range(1, int(n**(1/2)+1)): # 제곱근만큼 반복
         if n%i == 0: 
            if i == n//i: # 제곱근일 경우 1개만 카운트
                cnt += 1
            else: # 아니면 2개씩 카운트
                cnt += 2
    if cnt > limit: # 카운트가 limit을 넘어가면 power 리턴
        return power
    return cnt

# solution
def solution(number, limit, power):
    result = 0
    # 함수 적용
    for i in range(1, number+1):
        plus_num = plus_number(i, limit, power)
        result += plus_num
    return result
```

여기서 `if i == n//i` 제곱근일 경우 1개만 카운트하는 부분은,

예를 들어 16일 때, 

1, 2, **4**, **4**, 8, 16 → 1, 2, **4**, 8, 16이 약수이므로 제곱근인 4의 경우 1개만 카운트하는 것이 아닌가 싶다!

[**개인정보 수집 유효기간**](https://school.programmers.co.kr/learn/courses/30/lessons/150370)

```python
def solution(today, terms, privacies):
    answer = []
    t = {}
    t_y, t_m, t_d = map(int, today.split('.')) # [2022,5,19]

    for term in terms: # {"A":6,"B":12,"C":3}
        x, y = term.split(' ')
        t[x] = int(y)
    
    for i, p in enumerate(privacies):
        pri = p[-1]
        y, m, d = p.split('.')
        y = int(y)
        m = int(m)
        d = int(d[:2])

        if m + t[pri] > 12:
            y = y + ((m + t[pri]) // 12)
            m = (m + t[pri]) % 12           
        if m + t[pri] <= 12:
            m = m + t[pri]
            
        d -= 1
        if d == 0:
            d = 28
            m -= 1
        
        if t_y > y:
            answer.append(i+1)
        elif t_m > m:
            answer.append(i+1)
        elif t_d > d:
            answer.append(i+1)
        
    return answer
```

어찌저찌 코드를 짜내고, 제출 전 테스트도 다 통과했는데

막상 제출을 하니 한 문제 빼고 전부 실패가 떴다. 뭐가 문제인 걸까? 😑

일단 다른 사람의 풀이를 보고 다른 방식의 풀이를 완전이해했다!

```python
def solution(today, terms, privacies):
    answer = []
    # 일 단위로 통일
    y, m, d = today.split('.')
    today = int(y)*12*28 + int(m)*28 + int(d)
    # 약관 유형과 개월수를 딕셔너리로 저장
    terms = {i[:1]:int(i[2:])*28 for i in terms}

    for i,p in enumerate(privacies):
        y, m, d = p.split('.')
        d, c = d.split()
        p = int(y)*12*28 + int(m)*28 + int(d)
        
        if p+terms[c] <= today:
            answer.append(i+1)

    return answer
```

위와 같이 일수 단위로 통일할 수 있단 사실을 처음 깨닫게 되었다.

우선 년, 월, 일로 나눈 후 일 단위로 통일을 해주고 모두 더한다.

그리고 terms로 들어오는 값은 딕셔너리로 저장해준다.

다음은 enumerate로 인덱스값과 privacies에 있는 원소를 빼주고,

마찬가지로 똑같이 단위를 통일한 후 모두 더한다. 

그리고 약관 유형에 맞게 일수를 더해주고, today보다 작으면 인덱스+1 을 리스트에 추가한다.

이 방법은 굳이 12월, 28일이 넘어가는 경우, 0일이 되는 경우 등 내가 맨 처음 시도했던 방법이라면 번거로웠을 것들을 신경쓰지 않고 오로지 수의 합산으로만 해결할 수 있는 간단함이 정말 좋은 방법같다.

**01/19**

[**신고 결과 받기**](https://school.programmers.co.kr/learn/courses/30/lessons/92334)

역시 카카오 문제는 문제를 읽는 것부터가 큰 난관이고, 고비인 것 같다!

문제가 요구하는 것을 정확히 알아내는 것이 정말 중요한 것 같다.

문제를 이해하고 보니, 생각보다 엄청 어려운 문제는 아니었다.

하지만 A라는 유저가 B라는 유저를 여러 번 신고한 경우, 무조건 1회로만 인정을 해줘야 하는데,

이 부분을 어떻게 해결하지? 잠깐 막혔다가 set을 사용하면 되겠구나! 깨달았더니 바로 문제가 풀렸다.

```python
def solution(id_list, report, k):
    # 신고한 사람과 신고받은 사람의 신고 횟수를 누적시킬 딕셔너리 생성
    report_dict = {id : 0 for id in id_list} # {"muzi":0,"frodo":0,"apeach":0,"neo":0}
    reported_dict = {id : 0 for id in id_list}
    
    # 한 유저의 동일 유저에 대한 신고 제거(중복 제거)
    report = set(report)
    
    for rep in report:
        give, take = rep.split(' ')
        reported_dict[take] += 1 # 우선 신고받은 사람에게 신고 횟수를 누적
        
    for rep in report:
        give, take = rep.split(' ')
        if reported_dict[take] >= k: # k번 이상 신고받은 사람의 경우 
            report_dict[give] += 1 # 그 사람을 신고했던 사람에게 메일 발송 횟수 카운트

    answer = list(report_dict.values())
    return answer
```

1. 신고한 사람이 메일을 받을 횟수를 담을 딕셔너리와, 신고를 받은 사람의 누적 신고 횟수를 담을 딕셔너리를 생성한다.
2. 이때, **중복을 제거**하기 위해서 `set(report)`를 해준다.
3. for문을 두 번 돌릴 것인데, **첫번째 for문은 신고를 받은 사람의 누적 신고 횟수를 카운트**해준다.
4. **두번째 for문은, k번 이상 신고 받은 사람을 신고했던 사람에게 메일 발송 횟수를 카운트**해준다.
5. 그리고 정답으로 메일 발송 횟수가 담긴 딕셔너리의 value값을 리스트로 변환해서 return해준다.
