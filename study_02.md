# 12월 3주

**12/13**

- **[과일장수](https://school.programmers.co.kr/learn/courses/30/lessons/135808)**
    
    ```python
    def solution(k, m, score):
        money = 0
        # score 정렬
        s_score = sorted(score)
    
        # 리스트의 길이를 m으로 나눴을 때 나머지
        v = len(s_score)%m
        
        # v(나머지)가 0이 아니면 v만큼 리스트의 0번째부터 원소를 삭제
        if v != 0:
            del s_score[:v]
            
        # m 스텝마다 원소를 가져온 리스트 생성 (각 상자당 최저 사과 점수만 담은 리스트)
        s = list(s_score[::m])
            
        # 최대 이익 계산 후 리턴
        for i in s:
            money += i*m
        return money
    ```
    

- **[옹알이 (2)](https://school.programmers.co.kr/learn/courses/30/lessons/133499)**
    
    ```python
    def solution(babbling):
        count = 0
        
        for bab in babbling:
            # 중복된 발음이 있으면 continue(건너뛰기)
            if 'ayaaya' in bab or 'yeye' in bab or 'woowoo' in bab or 'mama' in bab:
                continue
            # 입력된 배열에 발음 가능한 단어가 있으면 공백으로 replace
            if 'aya' in bab:
                bab = bab.replace('aya', ' ')
            if 'ye' in bab:
                bab = bab.replace('ye', ' ')
            if 'woo' in bab:
                bab = bab.replace('woo', ' ')
            if 'ma' in bab:
                bab = bab.replace('ma', ' ')
            # 공백을 제거했을 때 문자열에 아무것도 없으면(발음이 가능한 단어) 카운트
            if bab.strip() == '':
                count += 1
        
        return count
    ```
    

**12/15**

- [**가운데 글자 가져오기**](https://school.programmers.co.kr/learn/courses/30/lessons/12903)
    
    ```python
    def solution(s):
        n = len(s)
        # 홀수면 가운데 한 글자 가져오기
        if n%2 != 0:
            return s[n//2]
        # 짝수면 가운데 두글자
        else:
            return s[n//2-1:n//2+1] # "qwer" -> s[1:3] -> "we"
    ```
    

- [**같은 숫자는 싫어**](https://school.programmers.co.kr/learn/courses/30/lessons/12906)
    
    ```python
    def solution(arr):
        # 맨 처음 0번째 원소는 꼭 들어가므로 answer에 배열의 0번째를 먼저 넣어둠
        answer = [arr[0]] 
        
        # arr의 원소 i가 answer의 마지막(-1번째) 원소와 중복되는지 확인
        for i in arr:
            # 중복이 아닐시 append
            if i != answer[-1]:
                answer.append(i)
        return answer
    ```
