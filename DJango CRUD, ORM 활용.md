### DJango CRUD

<hr>

## MODEL 기초


1. board app 설정

   ```bash
   $ python manage.py startapp boards
   ```

   

2. app 등록

   ```python
   # settings.py
   INSTALLED_APPS = [
       #...
       'boards',
   ]
   ```

   

3. Model 생성

   ```python
   #boards/models.py
   from django.db import models
   
   # Create your models here.
   class Board(models.Model): #네이밍은 단수로 표현
       title = models.CharField(max_length=10)
       content = models.TextField()
       created_at = models.DateTimeField(auto_now_add=True)
       updated_at = models.DateTimeField(auto_now=True)
       
       def __str__(self):
           return f'{self.id}: {self.title}'
   ```

   * DB의 컬럼과, 어떠한 타입으로 정의할 것인지에 대해 `django.db.models`를 활용해서 `Board`				클래스를 만든다.

   

4. Migration 파일 생성

   ```bash
   $ python manage.py makemigrations
   ```

   ```
   boards/migrations/0001_initial.py
       - Create model Board
   ```

   * DB에 반영하기 전에, 현재 등록된 APP의 `models.py`를 바탕으로 DB 설계도를 작성한다.\

   * `migrations` 폴더에 `0001_initial.py` 파일들이 생성된다.

   * `id` : Primary Key는 기본적으로 처음 테이블 생성 시 자동으로 만들어진다.

     ```python
     class Board(models.Model):
     	id = models.AutoField(primary_key=True)
         # ...
     ```

     

5. DB에 반영

   실제 쿼리문을 확인하는 방법

   ```bash
   $ python manage.py sqlmigrate boards 0001
   ```

   ```bash
   $ python manage.py sqlmigrate boards 0002
   ```

   ```
   BEGIN;
   --
   -- Create model Board
   --
   CREATE TABLE "boards_board" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "title" varchar(10) NOT NULL, "content" text NOT NULL, "created_At" datetime NOT NULL);
   COMMIT;
   ```

   * App이름 (boards)와 migration버전 (0001,0002, ..) 으로 명령어를 입력하면, 실제 데이터베이스에 적용되는 sql 쿼리를 확인할 수 있다.
   * `sqlmigrate` 명령어는 실제로 DB에는 반영되지 않는다.

   ```bash
   $ python manage.py migrate
   ```

   ```
     Applying contenttypes.0001_initial... OK
     Applying auth.0001_initial... OK
     Applying admin.0001_initial... OK
     Applying admin.0002_logentry_remove_auto_add... OK
     Applying admin.0003_logentry_add_action_flag_choices... OK
     Applying contenttypes.0002_remove_content_type_name... OK
     Applying auth.0002_alter_permission_name_max_length... OK
     Applying auth.0003_alter_user_email_max_length... OK
     Applying auth.0004_alter_user_username_opts... OK
     Applying auth.0005_alter_user_last_login_null... OK
     Applying auth.0006_require_contenttypes_0002... OK
     Applying auth.0007_alter_validators_add_error_messages... OK
     Applying auth.0008_alter_user_username_max_length... OK
     Applying auth.0009_alter_user_last_name_max_length... OK
     Applying boards.0001_initial... OK
     Applying boards.0002_auto_20190218_1519... OK
     Applying sessions.0001_initial... OK
   ```

   




## 장고  ORM 활용하기

> 시작하기에 앞서서 django shell 에서 기본 메소드를 활용하여 데이터베이스 조작을 해보자.

1. `django shell` 활용하기

   ```bash
   $ python manage.py shell
   ```

   * 이 경우에는 내가 활용할 모듈 혹은 파일을 직접 import 해야한다. 번거로우니까 `django_extensions`를 설치하여 활용하자.

     ```bash
     $ pip install django_extensions
     ```

   * 해당 모듈을 활용하기 위해서는 `settings.py`에 APP  등록을 해야한다.

     ```python
     # settings.py
     INSTALLED_APPS = [
         # ... 
         'django_extensions',
         'boards',
     ]
     ```

   * `shell_plus` 실행!

     ```bash
     $ python manage.py shell_plus
     ```

     * 실행하면,  `INSTALLED_APP` 에 설정된 내용들이 자동으로 import 된다.

2. 메소드 정리

   1. CRUD - C

      ```python
      # 첫 번째 방식
      board = Board()
      board.title = '1번제목'
      board.content = '1번내용'
      board.save()
      
      # 두 번째 방식
      board = Board(title='1번제목', content='1번내용')
      board.save()
      
      # 세 번째 방식
      board = Board.objects.create(title='1번제목', content='1번내용')
      ```

      * save()

        ```python
        board = Board(title='1번제목', content='1번내용')
        board.id #=> None
        board.created_at #=> None
        board.save()
        board.id #=> 1
        board.craeted_at #=> datetime.datetime(2019, 2, 18, 16, 20)
        ```

        * `save()` 메소드를 호출해야. DB에 저장된다. DB에 저장되면서 `id` 와 `created_at` 에 값이 부여된다.
        * `save()` 전에 `full_clean()` 메소드를 통해 현재 board 객체가 validation(검증)에 적합한지를 알아볼 수 있다.

   2. CRUD - R

      1. `all()`

         ```python
         boards = Board.object.all()
         #=> <QuerySet [<Board: 1:안녕?>, <Board: 2:2번글>, <Board: 3:3번글>]>
         ```

      2. `get(pk=value)`

         ```python
         Board.objects.get(pk=1)
         # Board.objects.get(id=1)와 동일
         #=> <Board: 1:안녕?>
         ```

         * `get()`은 데이터베이스에 일치하는 값이 없으면,  오류가 발생한다.
         * 또한, 결과가 여러개의 값이면, 오류가 발생한다.
         * 따라서 `id` 즉, Primary Key에만 사용하자
         * 리턴값은 board 오브젝트이다 (`filter()`,`all()`은 모두 queryset이 리턴된다.)

      3. `filter(column=value)`

         ```python
         Board.objects.filter(title='안녕?')
         Board.objects.filter(id=1)
         #=> <QuerySet [<Board: 1:안녕?>]>
         
         # 더블언더스코어(__) 활용
         Board.objects.filter(title__contains="번글")
         #=> <QuerySet [<Board: 2:2번글>, <Board: 3:3번글>]>
         ```

         * 데이터베이스에서 찾았을 때, 결과가 하나이더라도 리턴값은 QuerySet이다. 결과가 없어도 비어있는 QuerySet을 리턴한다.

   3. CRUD - D

      ```python
      board = Board.objects.get(pk=1)
      board.delete()
      #=> (1, {'boards.Board': 1})
      ```

   4. CRUD - U

      ```python
      board = Board.objects.get(pk=1)
      board.title = '수정수정'
      board.save()
      ```

      * `save()` 메소드는 board 오브젝트에 id가 없을 때에는 값을 추가하고, 있으면 수정한다.

   5. 추가 메소드

      ```python
      # 정렬
      Board.objects.order_by('title') #오름차순
      Board.objects.order_by('-title') #내림차순
      
      # 특정 단어 기준 탐색
      Board.objects.filter(title__contains='글') # title에 '글'이 들어간 모든 데이터
      Board.objects.filter(title__startswith='1') # title의 시작이 1인 모든 데이터
      Board.objects.filter(title__endswith='1') # title의 끝이 1인 모든 데이터
      Board.objects.filter(title__endswith='1')[0] # 0번째 데이터.. 1번째..
      ```

   6. ADMIN 관리

      ```bash
      $ python manage.py createsuperuser
      ```

      * 사용자이름, 이메일, 암호 입력후 서버를 실행시킨 뒤 /admin에 접속하면 된다.

   