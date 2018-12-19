# DAY3

## 1. Dictionary

> Dictionary는 `key`와 `value`가 짝지어져 있습니다.
>
> key는 `string`, `integer`, `boolean` 가능하다. `list` 혹은 `dictionary`는 안된다.
>
> value는 모든 자료형이 가능하다. `list` 혹은 `dictionary`도 가능하다.

1) dictionary 만들기

```python
phonebook = {
    "중국집":"2-2"
}
phonebook2 = dict(중국집=01)
```

2) 딕셔너리 조작하기

```python
phonebook["분식집"] = "031"
```

3) 딕셔너리

```python
idol = {
    "bts" : {
        "지민" : 23,
        "RM" : 24
    }
}
# 랩몬스터의 나이는? 
idol["bts"]["RM"]
```

4) 딕셔너리 반복문 활용하기

```python
# 기본 활용
for key in phonebook:
    print(key)
    print(phonebook[key])

# .items() 
for key, value in phonebook.items():
    print(key, value)
    
# value만 가져오기
for value in phonebook.values()
	print(value)
    
# key만 가져오기
for key in phonebook.keys()
	print(key)   
```



### [연습문제](https://zzu.li/dj_dict1)

```python
# 1. 평균을 구하세요.
iu_score = {
    "수학": 80,
    "국어": 90,
    "음악": 100
}

# 답변 코드는 아래에 작성해주세요.
print("=====Q1=====")
total_score = sum(iu_score.values())
print(total_score/len(iu_score))

# 2. 반 평균을 구하세요.
score = {
    "iu": {
        "수학": 80,
        "국어": 90,
        "음악": 100
    },
    "ui": {
        "수학": 80,
        "국어": 90,
        "음악": 30
    }
}
# 답변 코드는 아래에 작성해주세요.
print("=====Q2=====")
total_score=0
length = 0
for person_score in score.values():
    for point in person_score.values():
        total_score += point
        length += 1
average_score = total_score / length
print(average_score)

# 3. 도시별 최근 3일의 온도 평균은?
"""
출력 예시)
서울 : 값
대전 : 값
광주 : 값
부산 : 값
"""
# 3-1. 도시 중에 최근 3일 중에 가장 추웠던 곳, 가장 더웠던 곳은?
city = {
    "서울": [-6, -10, 5],
    "대전": [-3, -5, 2],
    "광주": [0, -2, 10],
    "부산": [2, -2, 9]
}

# 답변 코드는 아래에 작성해주세요.
print("=====Q3=====")
avg=0
for name, temp in city.items():
    avg= sum(temp)/len(temp)
    print(f"{name} : {avg}")

# 답변 코드는 아래에 작성해주세요.
print("=====Q3-1=====")
cold = 0
hot = 0
cnt = 0
hot_city=""
cold_city=""
for name, temp in city.items():
    # 첫 번째 시행 때,
    # name = "서울"
    # temp = [-6, -10, -5]
    if cnt == 0:
        hot = max(temp)
        cold = min(temp)
        hot_city = name
        cold_city = name
    else:
        #최저 온도가 cold 보다 더 추우면, cold에 넣고
        if min(temp) < cold:
            cold = min(temp)
            cold_city = name
        #최고 온도가 hot 보다 더 더우면, hot에 넣자
        elif max(temp) > hot:
            hot = max(temp)
            hot_city = name
    cnt += 1

print(hot_city)
print(cold_city)

# 4. 위에서 서울은 영상 2도였던 적이 있나요?
# 답변 코드는 아래에 작성해주세요.
print("=====Q4=====")
if 2 in city["서울"]:
    print("있어")
else:
    print("없어")
```



## Telegram  API 활용하기

> 사전 준비사항 : @botfather 를 통해서 봇을 만들어서 토큰 정보를 기록한다.

### 1) 봇 정보 가져오기

```python
f"https://api.telegram.org/bot{token}/getMe"
```

```python
import requests
token = "톡흔"
url = f"https://api.telegram.org/bot{token}/getMe"
response = requests.get(url)
```



### 2) 메세지 보내기

(1) user의 `id`를 가져와야함.

```python
https://api.telegram.org/bot{token}/getUpdates
```

```python
import requests
# 1. 사용자 정보 가져오기 위한 요청
token = "토큰"
url = f"https://api.telegram.org/bot{token}/getUpdates"
response = requests.get(url).json()

# 2.사용자 정보 및 메세지 설정
user_id = response["result"][0]["message"]["from"]["id"]
msg = "쓸 메시지 입력하면 된다."

# 3. 메세지 보내기
send_url = f"https://api.telegram.org/bot{token}/sendMessage?text={msg}&chat_id={user_id}"
requests.get(url)
```

