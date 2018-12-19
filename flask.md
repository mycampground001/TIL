# Flask

* [공식홈페이지](http://flask.pocoo.org/)

## 1. 설치하기

```
$ pip install flask
```

## 2. 시작하기

```python
# hello.py
from flask import Flask
app = Flask(__name__)

# "/" 루트 디렉토리를 뜻하며, www.naver.com
@app.route("/")
def hello():
    return "Hello World"
```

서버 실행하기

```python
$ FLASK_APP=hello.py flask run
```

크롬으로 `http://localhost:5000` 또는 `http://127.0.0.1:5000` 을 열어본다.

```python
@app.route("/ssafy")
def ssafy():
    return "hello, ssafy"
#http://localhost:5000/ssafy
```

## 3. Variable Routing

* url에 있는 정보를 변수로 활용하는 법

```python
@app.route("/greeting/<string:name>")
def greeting(name):
    return f"{name} 안녕?"
#http://localhost:5000/greeting/pigeon
#화면에서 pigeon 안녕? 출력
```

## 4. render_template

* `render_template`를 활용하기 위해서는 `import`를 해주어야 함

```python
#hello.py
from flask import Flask, render_template

@app.route("/")
def hello():
    return render_template("index.html")
```

```html
<!-- templates/index.html -->
<html>
    <head>
        
    </head>
    <body>
        안녕!?~
    </body>
</html>
```

* `render_template`을 사용하면 html 파일에서 변수, 조건, 반복문 등을 활용할 수 있다.
* `jinja2`라고 하는 템플릿 엔진을 활용하고 있기 때문이다.
* 기본적으로 html 파일은 `templates/` 폴더 안에 만들어야 한다.

```python
@app.route("/dinner")
def dinner():
    menu = "신선떡볶이"
    return render_template("dinner.html",menu=menu)
```

```html
<!-- templates/dinner.html -->
{{menu}} <!-- 출력을 원할 경우 {{ }}을 활용 한다.-->
{% if __조건__ %} <!-- 제어문을 활용할 경우 {% %}를 활용한다.
	<h1>True이면 보인다</h1>
{% else %}
	<h1>False이면 보인다</h1>
{% endif %}


{% for i in menus %}
	<p>{{i}}</p>
{% endfor %}
```
