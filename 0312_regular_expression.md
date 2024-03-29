# 0312_자연어처리 (정규표현식)

**컴퓨터 언어** : 

- 모든 텍스트를 전부 동일한 중요도로 인식 
- books와 book을 완전히 다르게 읽음. (복수, 단수 / 대문자, 소문자)
- 정규표현식 없이는 텍스트를 처리하는 것 비효율적



#### 정규표현식

특정 규칙을 가진 문자열의 집합을 간단하게 표현하는 식 

​	regular expression => regex / regexp 



#### 메타 문자

| 메타 문자 | 설명                                                         | ex                                                           |
| --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [ ]       | 괄호 안의 문자들 중 하나와 매치                              | [abc]<br />[a-z]<br />[ㄱ-ㅎ]                                |
| .         | \n 제외한 모든 '문자'와 매치 <br />문자열과 매치 아님 => 문자 개수 . 개수와 같게 |                                                              |
| *         | * 앞 문자 반복 (0개 포함)                                    | ca*t : ct, cat, caat                                         |
| +         | + 앞 문자 반복 (1개부터)                                     | ca+t : cat, caat                                             |
| ( )       | 그룹 내에 공통으로 들어가 있는 패턴으로 묶음                 | (tom\|pot)ato ⇒ tomato, potato                               |
| {m,n}     | 반복 횟수 고정.                                              | {1, 3} : 1이상 3 이하 반복 <br /> {3,} : 3 이상 <br /> {, 3} : 3 이하 |
| ?         | 있거나 없거나                                                | ab?c : ac \| abc                                             |
| ^         | 문자열의 시작                                                | ^http : http로 시작하는 경우                                 |
| $         | 문자열의 끝                                                  | com$ : com으로 끝나는 경우                                   |
| \         | escape character<br />메타 문자를 문자 그대로 인식하고 싶을 때 |                                                              |
| \|        | OR                                                           |                                                              |
| \b, \B    | 단어 경계 / 단어 경계 아닌 것                                |                                                              |
| \s, \S    | 공백 / 공백 아닌 것                                          |                                                              |
| \d, \D    | 숫자 / 숫자 아닌 것                                          |                                                              |
| \w, \W    | 단어 만들 수 있는 것, 문자로 인식하는 것 (숫자, 언더스코어, 공백 등) |                                                              |
| \n        | 개행 문자                                                    |                                                              |



## 실습

**import re** : python에서 정규표현식을 지원하는 라이브러리

- 패턴 생성 (정규표현식 이용) :

  ```python
  pat = re.compile()
  ```

  | 함수                          | 설명                                                         | ex                                                           | output                                                       |
  | ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | **re.search()**               | 문자열 전체에서 패턴과 매치되는 부분이 있으면 **match object** return | sample1 = 'abcd' / sample2 = 'ab'                            | sample1 :  **<re.Match object; span=(0, 3), match='abc'>** / sample2 :  **None** |
  | **re.match()**                | 문자열이 패턴으로 **시작**되면 match object return           |                                                              | search와 같음                                                |
  | **re.split()**                | 문자열 분리 : 파이썬의 기본 split과 달리 separator를 여러 개로 저장하는 것이 쉽다 | **re.split("[+=]", sample5)**   : + 또는 =를 separator로 사용 | list 리턴                                                    |
  | **re.findall()**              | 매치되는 모든 문자열의 리스트 반환                           |                                                              |                                                              |
  | **re.sub(a, b, c)**           | a를 b로 c 문자열 전체에서 바꿈                               |                                                              |                                                              |
  | **re.finditer()**             | findall과 비슷하지만 문자열의 리스트 대신 match object로 반환 |                                                              |                                                              |
  | **re.fullmatch(pat, string)** | 패턴과 문자열이 완벽하게 일치하면 match object 리턴 a = pattern / b = 문자열 |                                                              |                                                              |

  

- match object 

  ⇒ match object 핸들링하는 함수들 사용 가능

  - res.group()  :  일치된 문자열 반환

  - res.start() : 일치된 문자열의 시작 위치 반환

  - res.end()  : 일치된 문자열의 끝 위치 반환

  - res.span() : 일치된 문자열의 시작과 끝 위치 튜플로 반환

    

- **HTML**

- URL 호출 :

```python
import requests
response = requests.get("URL")

# 사용할 text 아래와 같이 얻을 수 있음
text = response.text
```

### text 전처리

1. 줄바꿈이나 탭 지우기
2. 비어있는 문자열 지우기
3. 문장들 하나로 합치기
4. punctuation 지우기
5. 소문자 혹은 대문자로 통일
6. 공백 기준으로 split

(개수별로 sort 등)

정도. 계속 확인하면서 지우고 싶은 부분 지우고 추가하고 싶은 부분 추가.

- 줄바꿈이나 탭 지우기 : **text = re.sub("[\n\t]", "", text)**

- ? : 최소매칭 ⇒ 가장 가까운 것 매칭

- filter(None, text) : 그냥 필터만 하면 filter 객체가 반환. 따라서 list로 불러줘야 함.

  ⇒ 비어있는 문자열 지우기

- 문장들을 하나의 text로 합치고 싶다면 join 함수 이용

  **' '.join(list)**  :  ' '을 중간에 두고 합쳐짐.

- punctuation 지우기 위해서는 \w가 아닌 것을 지우면 됨. (\W인 것)

### Beautifulsoup

데이터 추출 쉽게 해주는 tool ... 추출 이후 전처리는 정규표현식 이용,

특정 class 이름을 가지고 출력을 할 수도 있음.