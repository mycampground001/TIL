# DAY 2

## 스크래핑 기초

### 1. 설치

```
$ pip install requests
$ pip install bs4
```

* `requests` : 요청을 대신 보내준다.

* `bs4` : 문서를 파싱하는 것을 도와준다 (파싱: 구문을 가져와서 조작 )


### 2. 코스피 지수 가져오기

* 네이버에서 증권 페이지를 요청한다.
* 페이지에서 내가 찾고 싶은 내용을 찾는다.

1) 원하는 사이트에 요청을 보낸다.

```python
import requests
from bs4 import BeautifulSoup
url = "https://finance.naver.com/sise/"
```

2) 그 결과를 response에 저장한다.

```python
response = requests.get(url)
```

3) 원하는 정보를 CSS selector를 활용하여 가져오기 (copy->copy selector)

```python
soup = BeautifulSoup(response.text,'html.parser')
```

* Select는 Css의 선택(selector)을 통해 찾을 수 있다.

* #KOSPI_now : id가 KOSPI_now인 것을 뜻함.
*  .up : class가 up인 것을 뜻함.
* css에서 id는 문서에서 하나, class는 여러개가 있을 수 있다.

```python
kospi = soup.select_one('#KOSPI_now')
print(kospi.text)
```



### 3.  HTML / CSS

#### 1. HTML

HTML은 HyperText Markup Language의 약자로, 웹 문서에서 활용이 된다.

웹 문서의 구조와 내용을 담당한다.

```html
<!Doctype html>
<html>
    <head>
        <!-- 문서의 정보를 담고 있다. -->
    </head>
    <body>
        <!-- 문서의 내용을 담고 있다. 실제로 브라우저에서 보이는 내용이 여기에 해당함. -->
    </body>
</html>
```

#### 2. CSS

CSS는 Cascading Style Sheets의 약자로, HTML과 같은 마크업 언어를 꾸며주는 역할을 한다.

꾸며주기 위해서 특정한 태그를 선택해야 하는데 이 때 활용도는 것이 CSS 선택자(Selector)라고 부른다.

* `id` : #
* `class` : .
* `tag` 

``` html
<html>
    <head>
        <style>
            #blue {
                color:blue;
            }
            .red {
                color:red;
            }
            p {
                color:skyblue;
            }
        </style>
    </head>
    <body>
        <p class="red">클래스 적용 </p>
        <p id="blue">아이디 적용 </p>
        <p>태그 적용 </p>
        <p id="blue class="red"">파란색 </p>
        <p style="color: red;">인라인 속성 </p>
    </body>
</html>
```

아래는 본인이 작성한 코드

```html
<!Doctype html>
<html>
    <head>
        <title>네</title>
        <style>
        /* 클래스가 red */
            .red{
                color:red;
            }
        /*id가 blue*/
            #blue{
                color: blue;
            }
        /* 태그가 p*/
            p {
                color : skyblue;
            }
        </style>
    </head>
    <body>
    
    <!-- 인라인 > id > class > tag -->
    <!-- 인라인(inline) 속성 -->
        <h1 id="blue" style = "color:red;">안녕? 제목이니? </h1>
        <h2 class = "red">으악 빨간색 </h2>
        <p id="blue" class = "red">빨간색?파란색?</p>
        <p id = "blue">퍼런색</p>
        <p> 하늘색?</p>
    </body>
</html>
```



### 4. 파일 조작

### 1. `os` 외장 함수

```python
import os 
# 1. 내가 원하는 위치로 이동 - cd
os.chdir(r'경로')
# 2. 해당 디렉토리 내에 있는 파일명 가져오기 -ls
files = os.listdir()
# 3. 파일명 바꾸기
for file in files:
    os.rename(file,"지원자"+file)
```



1) SSAFY_지원자 폴더로 들어가기

```python
os.chdir(r'SSAFY_지원자')
os.chdir(r'SSAFY지원자')
print(os.getcwd()) #현재 파일위치표시
```

2)  내용 모두 출력해보기

```python
files = os.listdir()
```

3) 이름 바꾸어 보자

```python
for file in files:
    os.rename(file,file.replace("SAMSUNG","SSAFY")) 
    #replace(a,b)는 a의 문자를 찾아 b로 변경
```



### 2. file 열어서 조작하기

#### 1) 기본 사용법

```python
with open("파일명", "모드") as file:     ##모드는 "w","r" 
    file.write("글써짐")

with open("파일명", "r") as file:
    line = file.read()
    print(line)
```

파일을 조작하는 모드는 다음과 같다.

* w: write (덮어쓰기)
* r: read (읽기)
* a: append (이어쓰기)

#### 2) 파일 읽기

```python
# 1. read() : 전체를 하나의 string으로 가져오는 것
line = file.read()

# 2. readline() : 한줄 씩 string으로 가져옴
line = file.readline()  # 첫 번째 줄
line = file.readline()  # 두 번째 줄

#3. readlines() : 전체를 하나의 list, element는 한 줄의 string이 됨
lines = file.readlines()
for line in lines:
    print(line)
```

