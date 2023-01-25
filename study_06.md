# 1월 4주

**01/25**

**[성격 유형 검사하기](https://school.programmers.co.kr/learn/courses/30/lessons/118666)**

이 문제는 보자마자 딕셔너리를 사용하면 좋겠다! 라는 생각이 들어서 성격 유형 지표의 순서대로 값이 0인 딕셔너리부터 생성했다. 

매우 비동의 ~ 비동의는 왼쪽 유형의 값(values)에 점수를 더하고, 동의 ~ 매우 동의는 오른쪽 유형의 값에 점수를 더해서 성격 유형 점수를 합산했다.

이제 남은 것은 최종적으로 성격 유형을 출력하는 일인데, 각각 대응하는 유형의 값을 서로 비교해서 answer에 더했다. 이때, 값이 같으면 사전순으로 먼저 오는 알파벳이 출력되게끔 해야 한다. 

```python
def solution(survey, choices):
    answer = ''
    type_dict = {"R":0, "T":0, "C":0, "F":0, "J":0, "M":0, "A":0, "N":0}
    
    for i, s in enumerate(survey): # s="AN", s[0]="A", s[1]="N"
        left = s[0] 
        right = s[1]
        if choices[i] == 1:
            type_dict[left] += 3
        if choices[i] == 2:
            type_dict[left] += 2
        if choices[i] == 3:
            type_dict[left] += 1
        if choices[i] == 5:
            type_dict[right] += 1
        if choices[i] == 6:
            type_dict[right] += 2
        if choices[i] == 7:
            type_dict[right] += 3
            
    if type_dict["R"] >= type_dict["T"]:
        answer += "R"
    if type_dict["R"] < type_dict["T"]:
        answer += "T"
        
    if type_dict["C"] >= type_dict["F"]:
        answer += "C"
    if type_dict["C"] < type_dict["F"]:
        answer += "F"
        
    if type_dict["J"] >= type_dict["M"]:
        answer += "J"
    if type_dict["J"] < type_dict["M"]:
        answer += "M"
        
    if type_dict["A"] >= type_dict["N"]:
        answer += "A"
    if type_dict["A"] < type_dict["N"]:
        answer += "N"
         
    return answer
```
