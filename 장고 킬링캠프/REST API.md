# REST API

자원:HTTP METHOD + 행위:URI(URL)  두 가지를 활용하여 요청을 보낼 것임

<https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html>

* URI는 정보의 자원을 표현해야 한다.

  ```
  1)  GET /api/v1/artists/ 가수 목록
  2)  GET /members/delete/1
  ```

  > 2)는 틀린표현 delete같은 행위의 표현이 들어가 있으면 안 된다.

* 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)로 표현

  2)를 HTTP Method를 통해 수정하면

  ```
  DELETE /members/1
  ```

  회원정보를 가져올 때는 GET, 
  회원 추가 시의 행위를 표현하고자 할 때 POST METHOD활용

* HTTP Method
  * C : POST : 해당 URI를 요청하면 리소스를 생성함
  * R : GET : 해당 리소스를 조회하고 해당 도큐먼트에 대한 정보를 가져옴
  * U : PUT/PATCH : 해당 리소스를 수정함
  * D : DELETE : 해당 리소스를 삭제
  
* 표현은 JSON



### POST MAN 다운로드

API 요청을 대신보내주는 `POSTMAN` 다운로드 

```bash
$ pip install djangorestframework
$ django-admin startproject music_api
$ python manage.py startapp musics
```

```python
#settings.py
INSTALLED_APPS = [
	'rest_framework'
	'musics',
]
```



### Artist : Music (1:N), Artist : Comment(1:N)

```python
# musics/models.py
class Artist(models.Model):
    name = models.TextField()
    
    def __str__(self):
        return self.name
        
class Music(models.Model):
    title = models.TextField()
    artist = models.ForeignKey(Artist,on_delete=models.CASCADE)
    
    def __str__(self):
        return self.title
    
class Comment(models.Model):
    content = models.TextField()
    music = models.ForeignKey(Music, on_delete=models.CASCADE)
```

```python
# musics/admin.py
from .models import Artist, Music
# Register your models here.
admin.site.register(Artist)
admin.site.register(Music)
```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
$ python manage.py createsuperuser
```



### ADMIN에서 추가

Artists 여러명, Musics 여러개 추가해놓기

추가해놓은것을 json 형식으로 긁어올예정



### API URL 추가

```python
# music_api/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/',include('musics.urls')),
]
```

```python
# musics/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('musics/',views.music_list)
    ]
```



## READ - GET 요청

### 뮤직 list를 만들어보자.

```python
# musics/views.py
from django.shortcuts import render
from rest_framework.decorators import api_view
from .models import Music

# Create your views here.

@api_view(['GET']) # GET요청만 보낼 것이다.
def music_list(request):
    musics = Music.objects.all()
```

musics폴더에 serializers.py 생성, 형태는 forms와 동일하다.

```python
# musics/serializers.py
from rest_framework import serializers
from .models import Music

class MusicSerializer(serializers.ModelSerializer):
    class Meta:
        model = Music
        field = ['id','title','artist']
```

views.py에 추가

```python
from rest_framework.response import Response
# music_list 함수 내
serializer = MusicSerializer(musics, many=True)
    return Response(serializer.data)
```

postman에서 GET:http://django-intro-pigeonking.c9users.io:8080/api/v1/musics/ 



### DETAIL 만들어보기

```python
# urls.py
path('musics/<int:music_pk>/',views.music_detail),
```

```python
#views.py
@api_view(['GET'])
def music_detail(request,music_pk):
    music = get_object_or_404(Music, pk=music_pk)
    serializer = MusicSerializer(music)
    return Response(serializer.data)
```



### 명세 만들어보기 (SWAGGER 활용)

```bash
$ pip install django-rest-swagger
```

```python
# settings.py에 추가
INSTALLED_APPS = [
	'rest_framework_swagger',]
```

```python
# musics/urls.py에 추가
from rest_framework_swagger.views import get_swagger_view
path('docs/', get_swagger_view(title='음악 정보 API')),
```

<http://django-intro-pigeonking.c9users.io:8080/api/v1/docs/> 들어가서 확인해보기



### docstring 

```python
def music_list(request):
    '''
    음악 정보 출력
    '''
    
def music_detail(request,music_pk):
    '''
    음악 상세 보기
    '''
```

docs에 주석이 추가된다.!



### Artist list 출력하기

Music list 출력과 똑같이 해주면 된다. 

serializers.py, urls.py, views.py 손 봐주면 된다.



### Artist list // 노래정보 딕셔너리로 출력

```python
# serializers.py 
class ArtistSerializer(serializers.ModelSerializer):
    musics = MusicSerializer(source='music_set', many=True, read_only=True)
    class Meta:
        model = Artist
        fields = '__all__'
```

```
[{ "id": 1, "musics": [...],"name":'아이유'},{ "id": 2, "musics": [...],"name": "future"}]
```



### Artist Detail

구분을 짓기 위해

```python
# serializers.py
class ArtistDetailSerializer(serializers.ModelSerializer):
    music_count = serializers.IntegerField(source='music_set.count')
    class Meta:
        model = Artist
        fields = '__all__'
        
class ArtistSerializer(serializers.ModelSerializer):
    musics = MusicSerializer(source='music_set', many=True, read_only=True)
    class Meta:
        model = Artist
        fields = '__all__'
```

```python
# views.py
@api_view(['GET'])
def artist_detail(request,artist_pk):
    '''
    가수 정보
    '''
    artist = get_object_or_404(Artist, pk=artist_pk)
    serializer = ArtistDetailSerializer(artist)
    return Response(serializer.data)
```



## Comment - POST 요청

### 댓글 만들기

```python
# serializers.py
class CommentSerializer(serializers.ModelSerializer):
    class Meta:
        model = Comment
        fields = ['content']
```

```python
# urls.py
path('musics/<int:music_pk>/comments/',views.comment_create),
```

```python
# views.py
@api_view(['POST'])
def comment_create(request, music_pk):
    serializer = CommentSerializer(data=request.data)
    if serializer.is_valid():
        serializer.save(music_id=music_pk)
        return Response(serializer.data)
```



POST 요청을 처리할 때는 Body를 쓴다. Params를 쓰면 ?가 붙어서 GET 요청이 된다.

POST MAN > POST > http://django-intro-pigeonking.c9users.io:8080/api/v1/musics/1/comments/ > SEND > Body > Key: content, Value: 댓글입력 > SEND

Content를 넣지 않으면 오류를 띄운다. 해결방법은

```python
# comment_create 함수의
if serializer.is_valid(raise_exception=True):
```

위와 같이 수정하면 "이 필드는 필수 항목입니다." 라고 뜸.



## UPDATE, DELETE - DELETE,PUT 요청

```python
# urls.py
path('musics/<int:music_pk>/comments/<int:comment_pk>/', views.comment_update_delete),
```

```python
# views.py
@api_view(['DELETE','PUT'])
def comment_update_delete(request,music_pk, comment_pk):
    comment = get_object_or_404(Comment, pk=comment_pk)
    if request.method == 'PUT':
        serializer = CommentSerializer(data=request.data, instance=comment)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response({'message':'성공적으로 수정 되었습니다.'})
    else:
        comment.delete()
    return Response({'message':f'음악 {music_pk}의 댓글이 삭제되었습니다.'})
```

POST MAN>PUT>http://django-intro-pigeonking.c9users.io:8080/api/v1/musics/1/comments/5/>VALUE 수정>SEND

POST MAN>DELETE>http://django-intro-pigeonking.c9users.io:8080/api/v1/musics/1/comments/5/>SEND