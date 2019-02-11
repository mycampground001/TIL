## Django 세팅하기

`python3 -V`,` python -V`를 통해 버전 확인

`zzu.li/c9` - c9_settings.txt 복사 붙여넣기 

__또는__ 

```python
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc

source ~/.bashrc
pyenv install 3.6.7
pyenv global 3.6.7
python -V
pip install --upgrade pip
pip install flask
pip install requests

git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc
exec "$SHELL"
```

다음으로 django 가상 환경을 만들기 위해 아래와 같이 복붙

```python
pyenv virtualenv django-venv
pyenv local django-venv
pip install django
```





MVC 패턴 Model, View, Controller

> Model = DataBase
>
> View = 보여지는 것들
>
> Controller = Model, View 간 연결고리



Django에서는 __MTV__

>Model
>
>Template
>
>View



### 장고 실행

```python
$ django-admin startproject django_intro
```

입력하면 `django_intro` 파일과 `manage.py`가 생성됨

django_intro 폴더 안 setting.py에서 아래와 같이 변경

```python
ALLOWED_HOSTS = ['*']
```

```python
$ cd django_intro
ls
python manage.py runserver 0.0.0.0:8080
```

로켓 뜨면 성공



`gitignore.io` 들어가서 `django` 입력후 `create` 다음

```bash
$ vi .gitignore
https://gitignore.io/api/django
```

esc 후 :wq 입력



zai settings.py

```python
INSTALLED_APPS = [...
                 ]
```

마지막 아이템에도 컴마가 붙어있음

아래와 같이 변경해주기.

```python
#LANGUAGE_CODE = 'en-us'
LANGUAGE_CODE = 'ko-kr'

#TIME_ZONE = 'UTC'
TIME_ZONE = 'Asia/Seoul'
```

ko-kr로 설정해주면 한글패치가 됨.



APP 만들기 명령어

```python
$ python manage.py startapp home
```

home이라는 폴더가 생성됨

이것들이 MTV로 구성되어있다.



#### 구현

url.py 에서 아래와 같이

```python
from django.contrib import admin
from django.urls import path
# home 폴더 내에 있는 views.py를 불러온다.
from home import views

urlpatterns = [
    path('admin/', admin.site.urls),
    # 요청이 home/으로 오면, views의 index 함수를 실행시킨다.
    path('home/',views.index)
]
```

=> flask의 app.route('/home') 같이 urlpatterns에 추가를 해준다. + from, import



home -> views.py에서 아래와 같이

```python
from django.shortcuts import render, HttpResponse

# Create your views here.

def index(request):
    return HttpResponse('hello, django!')
```

=> flask의 render_template('index.html') 같이 views.py에서 수정해준다.



다음 settings.py INSTALLED_APPS =[...] 아래와 같이.

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'home',
]
```

home을 추가해준다.



GET/ home/dinner/  -> 저녁메뉴를 추천한다.

1. 요청 url 설정

   ```python
   from home import views # home은 파일명
   path('home/dinner/',views.dinner)
   ```

2. view 설정

   ```python
   def dinner(request):
       box = ['a','b','c','d']
       dinner = random.choice(box)
       return render(request, 'dinner.html',{'dinner':dinner})
   ```

   - Template을 리턴하려면, `render` 를 사용하여야 한다.
     - `request`(필수)
     - `template 명` (필수)
     - `template에서 쓸 변수` (선택) : {'item': item} : `dictionary` 타입으로 구성해야 한다.

3. Template 설정

   ```bash
   $ mkdir home/templates
   $ touch home/templates/dinner.html
   ```

   ```html
   <!-- home/templates/dinner.html-->
   <h1>
       {{dinner}}
   </h1>
   ```

   