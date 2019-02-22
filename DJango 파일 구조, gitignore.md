```
├── manage.py
├── django_intro
	├── urls.py
    ├── settings.py
    └── templates
        └── base.html
     
├── home
    ├── migrations
    ├── static
    │   └── home
    │       └── image
    │       └── css
    ├── templates
    │   └── home
    │       └── user_read.html
    ├── urls.py
    └── views.py

└── utilities
    ├── migrations
    ├── static
    │   └── utilities
    │       └── image
    │       └── css
    ├── templates
    │   └── utilities
    │       ├── ascii_make.html
    ├── urls.py
    └── views.py
    
└── other
    ├── migrations
    ├── static
    │   └── utilities
    │       └── image
    │       └── css
    ├── templates
    │   └── utilities
    │       ├── other_go.html
    ├── urls.py
    └── views.py 
```



### 1.  main

```
├── django_intro
	├── urls.py
    ├── settings.py
    └── templates
        └── base.html
```

```python
# settings.py
TEMPLATES = 
    'DIRS': [os.path.join(BASE_DIR, 'django_intro', 'templates')],
ROOT_URLCONF = 'django_intro.urls'
INSTALLED_APPS = [
    'home',
    'utilities',
    'other',]
ALLOWED_HOSTS = ['*']
LANGUAGE_CODE = 'ko-kr'
TIME_ZONE = 'Asia/Seoul'
STATIC_URL = '/static/'
```

```python
#urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('home/',include('home.urls')),
    path('utilities/',include('utilities.urls')),
    path('other/',include('other.urls'))
]
```


```html
<!--base.html-->
<head>
<title>{% block title %} {% endblock %}</title>
    {% block css %}{% endblock %}
</head>

<body>
<nav bar...></nav>
{% block body %}{% endblock %}
</body>
```



### 2. home, utilities, other

```python
# urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('',views.index),
    path('user_read/',views.dinner),]
```

```python
# views.py
def user_read(request):
    username = request.POST.get('username')
    password = request.POST.get('password')
    return render(request, 'home/user_read.html',{'username':username,'password':password})
```

```django
<!--.html-->
{% extends 'base.html' %} or {% extends 'home/base.html' %}
{% load static %}

{% block css %}
<link rel="stylesheet" href="{% static 'home/css/style.css' %}" type="text/css" />
{% endblock %}

{% block body %}
    <form action="/user_read/" method="POST">
        {% csrf_token %} <!--POST 사용시 옆의 토큰을 써야됩니다.-->
        이름 <input type="text" name="username">
        암호 <input type="password" name="password">
        <input type="submit">
    </form>
	<img src="{% static 'home/image/flare.jpg' %}">
{% endblock %}
```

```
http://django-intro-pigeonking.c9users.io:8080/home/user_read
http://django-intro-pigeonking.c9users.io:8080/home/...

http://django-intro-pigeonking.c9users.io:8080/utilities/ascii_make
http://django-intro-pigeonking.c9users.io:8080/utilities/...

http://django-intro-pigeonking.c9users.io:8080/other/other_go
http://django-intro-pigeonking.c9users.io:8080/other/...
```



### 장고 git 파일 관리

```bash
$ git init
$ vi .gitignore
```

https://gitignore.io/api/django 의 내용을 전부 복사해서 INSERT 붙여넣기 다음 esc + :wq

```bash
$ git add .
$ git commit -m '...'
```

