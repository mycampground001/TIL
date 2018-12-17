# Day1

## 1. CLI(command Line Interface)

 명령어를 통해서 사용하는 인터페이스로, 기존의 GUI(Graphic User Interface)와는 다르게 터미널(bash/shell/cmd)을 통해서 명령을 할 수 있다.

 사전 준비사항 : [Git bash](https://gitforwindows.org)  설치

## 1. 기본 명령어

```
$ 1s
```



## 2. Python

### 0) Python Style guide(PEP-8)

[공식문서링크](https://www.python.org/dev/peps/pep-0008/)

###     1) string 조작

```python
# 기본 방법
print("안녕하세요?")
print("저는 홍길동 입니다.")
print("만나서 반갑습니다.")

print("""안녕하세요 
나는 홍길동입니다.
만나서 반갑습니다.
""")
```

출력결과 

```
안녕하세요
저는 홍길동 입니다.
만나서 반갑습니다.

안녕하세요
나는 홍길동입니다.
만나서 반갑습니다.
```



### (2) String InterPolation

1. f-string

   ```python
   name = "홍길동"
   print(f"안녕하세요, {name}입니다.")
   #=> "안녕하세요, 홍길동입니다.
   ```



2. [pyformat](https://pyformat.info/)

   ```python
   name = "홍길동"
   english_name = "hong"
   print("안녕하세요, {}입니다. My name is {}".format(name, english_name))
   #=>안녕하세요. 홍길동입니다. Myname is hong
   print("안녕하세요, {}입니다. My name is {0}".format(name, english_name))
   #=>안녕하세요. 홍길동입니다. Myname is hong
   print("안녕하세요, {}입니다. My name is {1}".format(name, english_name))
   #=>안녕하세요. 홍길동입니다. Myname is hong
   ```



3. 모르면 이렇게 하자

   ```python
   name = "홍길동"
   print("안녕하세요," + name + "입니다.")
   ```



## 2) range

   `range`는 숫자의 범위를 가지고 있는 시퀀스다.

   ```python
   print(range(5))
   #=> range(0,4)
   # list 형변환
   a = list(range(5))
   print(a)
   #=> [0, 1, 2, 3, 4]
   
   for i in range(5):
       print(i)
   #=> 0
   #=> 1
   #=> 2
   ```


## 3) List

`list`는 배열 또는  array라고도 불린다. 인덱스를 통해 값에 근접할 수 있다.

```python
menu = ["중국집", "초밥", "고기", "분식"]
menu[0]
#=> 중국집
```



## 4) Dictionary

`Dictionary` 는  hash라고도 불린다.`key` 와  value가 짝지어져 있다.

``` python
phonebook = {
    "중국집" : "123-456",
    "초밥" : "4342-111",
    "고기" : "213-4123"
    "분식" : "565-111"
}
phonebook["분식"]
#=> 565-111
```



# 3.[마크다운(Markdown)](https://www.markdownguide.org/)
## 1. Heading

```
# H1입니다.
## H2입니다.
### H3입니다.
#### H4입니다.
##### H5입니다.
```

# H1입니다.
## H2입니다.
### H3입니다.
#### H4입니다.
##### H5입니다.



### 2.List

```
* 순서 없는 리스트
* 순서 없는 리스트

1. 순서 있는 리스트1
2. 순서 있는 리스트2
3. 순서 있는 리스트3
```

* 순서 없는 리스트
* 순서 없는 리스트

1. 순서 있는 리스트1
2. 순서 있는 리스트2
3. 순서 있는 리스트3



### 3. 코드 작성(Code snippet)

```python
​```python
print("hello, world")
​```
```

```python
print("hello, world")
```



### 4. 링크 연결

```
[구글로 가는 링크](https://google.com)
```

[구글로 가는 링크](https://google.com)



### 5. 글씨 꾸미기

```
_기울임_
*기울임*
**굵게**
__굵게__
```

*안녕*

**안녕**

***굵게***



### 6. 기타

```
---
***
> 안녕하세요?
```

> 안녕하세요?
>
> ---
>
> 인용문 공간입니다.
>
> ***
>
> 안녕?

