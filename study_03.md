# 12월 5주

**12/29**

[**소수 찾기**](https://school.programmers.co.kr/learn/courses/30/lessons/12921)

```python
def solution(n):
    answer = []
    for i in range(2, n+1): # 1은 소수가 아니므로 2부터 시작
        check = True
        # for문으로 i가 1과 자신이 아닌 숫자로 나뉘어지면 false
        for v in range(2, i):
            if i % v == 0:
                check = False
                break
        # false가 아닌 소수들은 리스트에 담고 len으로 그 개수를 리턴
        if check != False:
            answer.append(i)
    return len(answer)
```

이 코드는 테스트 케이스는 통과하였으나, 제출하면 시간 초과로 실패한다.

이중 for문이라 시간이 걸리는 것 같음!

알고리즘 자료구조책의 소수 찾기 파트를 보면 자세한 과정이 설명되어있는 듯 한데, (97pg~)

봐도 눈에 들어오지 않아서 전혀 이해를 못하고 있다. 당최 코드를 어떻게 줄여야 할지 모르겠다😅

![image](https://user-images.githubusercontent.com/106129152/209951728-5009ba7a-3f2d-4a88-951d-6d4eb930ff81.png)

아래는 다른 사람의 풀이!

2부터 n+1 까지 집합을 만든 후, 2부터 n까지 반복문을 돌린다.

여기서 i가 집합에 있다면 집합에서 i의 배수를 제외시켜버린다.

이렇게 해서 남아있는 소수의 개수를 반환하는 것이다!

정말 깔끔명료한 코드라 놀랐다.

```python
num = set(range(2, n+1))
    for i in range(2, n+1):
        if i in num:
            num -= set(range(2*i, n+1, i))
    return len(num)
```

[**시저 암호**](https://school.programmers.co.kr/learn/courses/30/lessons/12926)

```python
def solution(s, n):
    answer = ''
    B = "A B C D E F G H I J K L M N O P Q R S T U V W X Y Z A B C D E F G H I J K L M N O P Q R S T U V W X Y Z".split()
    b = [i.lower() for i in B] # B의 소문자 리스트
    for i in s:
        # 공백은 그냥 추가
        if i == ' ':
            answer += i
        else:
            # i가 소문자일 때
            if i.islower():
                y = b.index(i)
                answer += b[y+n]
            # i가 대문자일 때
            elif i.isupper():
                x = B.index(i)
                answer += B[x+n]     
    return answer
```

이 문제는 정말 단순무식하게 풀었다. 솔직히 쓰면서도 통과가 될 줄 몰랐는데 통과했다(…)

입력된 문자 s에서 개별 원소인 i를 뽑아낸 후, 소문자/대문자별로 나눈 리스트에서 i의 인덱스 번호를 따로 뽑아낸다. 이 i의 인덱스 + n 으로 다시 색인을 해준다.

이 코드의 단점은, ***n에 제한조건이 없어지면 리스트를 무작정 늘리기만 할 수 없으므로 한계***가 있다.

→ 해결책으로, 굳이 a~z를 추가하지 않고, 색인할 때 **[(i의 인덱스 + n) % 26] 로 append**하는 방법이 있다.

다른 사람들의 풀이를 보면 **chr, ord** 라는 함수를 사용했는데, 처음 보는 함수라 검색을 해봤다.

- **chr()** : 정수인 유니코드를 넣으면 그 코드에 해당하는 문자열 리턴
- **ord()** : chr()의 반대. 문자열을 넣으면 그에 맞는 유니코드(정수)를 리턴


다시 보완한 코드

```python
def solution(s, n):
    answer = ''
    B = "A B C D E F G H I J K L M N O P Q R S T U V W X Y Z".split()
    b = [i.lower() for i in B] # B의 소문자 리스트
    for i in s:
        # 공백은 그냥 추가
        if i == ' ':
            answer += i
        else:
            # i가 소문자일 때
            if i.islower():
                y = b.index(i)
                answer += b[(y+n)%26]
            # i가 대문자일 때
            elif i.isupper():
                x = B.index(i)
                answer += B[(x+n)%26]     
    return answer
```


