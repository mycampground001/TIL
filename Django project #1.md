## Django project #1

```bash
$ python manage.py startapp utilities
```

settings.py -> 'utilities', 추가



home 폴더 내 __urls.py__ 파일 생성

```python
#워크스페이스 폴더 내
from django.urls import path, include
# home 폴더 내에 있는 views.py를 불러온다.
from home import views
from utilities import views

urlpatterns = [
    path('admin/', admin.site.urls),
    # 요청이 home/으로 오면, views의 index 함수를 실행시킨다.
    path('home/',include('home.urls'))
    path('home/',views.index),
    path('home/dinner/',views.dinner),
    path('home/cube/<int:num>',views.cube),
    path('home/ping/',views.ping),
    path('home/pong/',views.pong),
    path('home/user_new/',views.user_new),
    path('home/user_read/',views.user_read),
    path('home/template_example/',views.template_example),
    path('home/static_example/',views.static_example),
]
```

17번째 줄부터 urls.py 파일에 복붙하기, 아래와 같이 수정

```python
# home 폴더 내
from django.urls import path
from . import views


urlpatterns = [
    path('',views.index),
    path('dinner/',views.dinner),
    path('cube/<int:num>',views.cube),
    path('ping/',views.ping),
    path('pong/',views.pong),
    path('user_new/',views.user_new),
    path('user_read/',views.user_read),
    path('template_example/',views.template_example),
    path('static_example/',views.static_example),
]
```

```python
# 워크스페이스 폴더 내
from django.contrib import admin
from django.urls import path, include
# home 폴더 내에 있는 views.py를 불러온다.

urlpatterns = [
    path('admin/', admin.site.urls),
    # 요청이 home/으로 오면, views의 index 함수를 실행시킨다.
    path('home/',include('home.urls')),
    path('utilities/',include('utilities.urls')),
]
```



다음으로 utilities 폴더에 urls.py 파일 생성 후 아래와 같이 작성

```python
from django.urls import path
# home 폴더 내에 있는 views.py를 불러온다.
from . import views

urlpatterns = [
    path('',views.index), #utilities/
]
```

그리고 views.py에 함수 작성하고 templates 폴더 생성 후 index.html 생성 후 서버 실행하면

```
Django 실습
장고장고장고
저녁메뉴 추천받기
```

utilities의 내용은 어디가고 위와 같이 home의 내용이 뜸 



먼저 home 플더 내 static, templates 폴더에서 home 폴더를 또 생성하고 내용물 home 에 넣기

```
app/templates/app/___ 구조화
```

다음 home -> views.py -> ctrl+f 후 ->request, ' 로 찾은 뒤 request, 'home/ 으로 replace



다음으로 서버 실행하면 <...>/utilities/ 성공 but /home/ 안됨

django_intro 폴더 내 templates 폴더 생성하고 base.html을 가져온다.

다음 settings.py에서 아래와 같이 수정하고 home/을 해본다. -> 성공

```python
TEMPLATES = [ ...
DIRS': [os.path.join(BASE_DIR, 'django_intro', 'templates')],            
             ...]
```

