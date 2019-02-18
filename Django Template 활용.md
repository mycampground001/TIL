## Django Template 활용

### views.py

```python
def template_example(request):
    my_dict = {'name':'kim','nickname':'edutak','age':100}
    my_list = ['자장면','짬뽕','탕수육','양장피']
    my_sentence = 'Life is short, you need python!'
    messages = ['apple','banana','cucumber','mango']
    datetimenow = datetime.datetime.now()
    empty_list = []
    return render(request,'template_example.html',{'my_dict':my_dict,'my_list':my_list,'my_sentence':my_sentence,'messages':messages,'empty_list':empty_list,'datetimenow':datetimenow})
```



### 반복문

```django
{% for menu in my_list %}
    {{ forloop.counter }} 
    {% if forloop.first %}
        <p>자장면+고춧가루</p>
    {% else %}
        <h4> {{ menu }}</h4>
    {% endif %}
{% endfor %}

{% for user in empty_list %}
    <p> {{user}}</p>
    {% empty %}
        <p> 지금 가입된 유저가 없습니다.</p>
{% endfor %}
```

```
## 결과 ##
1
자장면+고춧가루
2
짬뽕
3
탕수육
4
양장피
지금 가입된 유저가 없습니다.
```

`{{ forloop.counter }}` : 숫자 출력

`{% if forloop.first %}` : 첫행만 

`{% empty %}` : 비어있으면의 조건문



### 조건문

```django
{% if '짜장면' in my_list %}
    <h4> abcabc</h4>
{% endif %}

<h3>length filter 활용</h3>
{% for message in messages %}
    {% if message|length > 5 %}
        <p>글자수가 5를 넘어갑니다.</p>
    {% else %}
        <p> {{message}}, {{message|length}}</p>
    {% endif %}
{% endfor %}
```

```
## 결과 ##
length filter 활용
apple, 5
안녕하세요
안녕하세요
mango, 5
```

`{% if message|length > 5 %}` : 길이가 5를 초과하면.



### lorem

```django
<h3> lorem ipsum </h3>
{% lorem %}
<hr>
{% lorem 3 w %}
<hr>
{% lorem 4 w random %}
<hr>
{% lorem 4 p %}
```

```
## 결과 ##
lorem ipsum
Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

lorem ipsum dolor

vero rem pariatur ipsa

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

Dolorum explicabo accusantium voluptatem? A totam quos, consequatur officia a architecto dolores aut velit. Eius quam corrupti rem ipsam atque necessitatibus dolor aliquam, accusamus fugit porro minus enim suscipit nesciunt laudantium voluptatem in quos.

Iusto repellendus numquam iure, incidunt adipisci blanditiis eos explicabo ipsum facere ducimus optio, id ex saepe laboriosam nobis.

Dolorem corporis qui inventore optio non amet eveniet.
```

`{% lorem 4 p %}` : 각 문단이 p태그로 감싸져 있다.

`{% lorem 3 w %}` : 단어 3개를 출력한다.

`{% lorem 4 w random %}` : 단어 4개를 랜덤으로 출력한다.



### 글자수 제한 (Truncate)

```django
<p>{{ my_sentence | truncatewords:3 }}</p>
<p>{{ my_sentence | truncatechars:10 }}</p>
```

```
Life is short, ...

Life i ...
```

`{{ my_sentence | truncatewords:3 }}` : 3단어 출력후 ... 출력

`{{ my_sentence | truncatechars:10 }}` : 문장의 길이가 ...을 포함해서 10이 된다.



### 글자 관련 필터

```django
<p>{{ 'ABC'| lower }}</p>
<p>{{ my_sentence| title }}</p>
<p>{{ my_sentence | capfirst }}</p>
<p>{{ my_sentence | length }}</p>
<p>{{ my_list | random }}</p>
```

```
abc
Life Is Short, You Need Python!
Life is short, you need python!
31
양장피
```

`{{ my_sentence | title }}` : 공백을 기준으로 첫 알파벳을 대문자로 바꾸어 준다.

`{{ my_sentence | capfirst }}`  : 첫 알파벳만 대문자로 바꾸어 준다.

`{{ my_sentence | length }}` : 길이를 출력한다.

`{{ my_list | random }}` : 리스트에서 랜덤으로 하나를 출력한다.



### 연산

```django
<p> {{ 4|add:6 }}</p>
```

`{{ 4|add:6 }}` : 4+6=10, 10을 출력한다.

기본적으로 add만 제공됨. 나머지 연산을 이용하고 싶으면 views.py에서 계산해서 넘기거나 `django-mathfilters`를 활용하면 된다.



### 날짜표현

```django
<p>{{now}}</p>
<p>{% now "SHORT_DATETIME_FORMAT" %}</p>
<p>{% now "DATETIME_FORMAT" %}</p>
<p>{% now "SHORT_DATE_FORMAT" %}</p>
<p>{% now "DATE_FORMAT" %}</p>
<p>{% now "Y년 m월 d일 (l) h:i" %}</p>
<p>{{ datetimenow|date:"SHORT_DATE_FORMAT"}}</p>
```

```
2019년 2월 12일 11:23 오전
2019-2-12 11:22
2019년 2월 12일 11:22 오전
2019-2-12.
2019년 2월 12일
2019년 02월 12일 (화요일) 11:24
2019-2-12.
```

날짜표현은 장고에서 views.py에서 now를 생성하지 않아도 템플릿으로 쓸 수 있다.!! 다른 날짜를 구할 때 써야한다면 import datetime, 해주면 된다.

datetimenow의 인자를 넘겨주었을 때 `{{ datetimenow|date:"SHORT_DATE_FORMAT"}}` 의 FORMAT 형태를 출력한다.



### URL

```django
{{ 'http://google.com'|urlize }}
```

a 태그로 생성