# ORM 

`sqlite <-> Mysql` 어떤 DB든 쿼리문을 바꾸지 않아도 된다. django는 ORM을 내장하지만 FLASK는 내장되어 있지 않아 SQL alchemy라는 ORM을 사용한다.

Flask sqlalchemy 접속 -> Quick Start 들어가서 복사하기 -> `'sqlite:///db_flask.sqlite3'` 변경 -> from flask_migrate import Migrate, migrate = Migrate(app, db) 작성하고 

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///db_flask.sqlite3'
db = SQLAlchemy(app)


migrate = Migrate(app, db)
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)

    def __repr__(self):
        return '<User %r>' % self.username
```

다음 `pip install flask_sqlalchemy flask_migrate` 를 통해 둘을 설치



`flask db init` 명령어 입력하면 `migrations` 폴더가 생성됨

다음 `flask db migrate` 명령어 입력 -> db_flask.sqlite3 파일 생성 : class 내용을 db에 반영하기 위해 준비

다음 `flask db upgrade`  명령어 입력 : migration 파일을 실제 db에 반영

다음 `sqlite3 db_flask.sqlite3` 입력 -> .table , .schema user 를 통해 user  테이블 생성되었는지 확인



(`flask shell` 명령어는 플라스크를 파이썬으로 사용)



활용

```
python -> from app import * 입력 -> User 입력 -> <class 'app.User'> 확인
-> user = User(username='재찬', email='lee@lee') 입력 후 user 입력 -> <User '재찬'>확인
->db.session.add(user)->db.session.commit()
user1 = User(username='승만', email='no@no') -> db.session.add(user1) -> db.session.commit() -> exit() -> sqlite3 db_flask.sqlite3 -> SELECT * FROM user; -> 입력한 것이 추가되었는지 확인
```

```
app.py class 내에서 password = db.Column(db.String(30)) 작성 -> flask db migrate -> flask db upgrade 입력 -> sqlite에서 .schema를 통해 password가 추가되었는지 확인.
```



다시 db조작해보기 -> migrate 폴더와, db_flask.sqlite3 파일삭제

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate
import datetime

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///db_flask.sqlite3'
db = SQLAlchemy(app)


migrate = Migrate(app, db)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    create_at = db.Column(db.String(80), nullable=False)
    
    def __init__(self,username,email):
        self.username = username    
        self.email = email
        self. create_at = datetime.datetime.now().strftime("%D")

    def __repr__(self):
        return f'{self.username}: {self.email}'
```

-> init, migrate, upgrade 명령어 입력 ->python -> 활용 다시 

-> users = User.query.all()

->User.query.filter_by(username='신욱').all()

->u1 = User.query.get(2)

->u1

->u1.username = '영'

->db.session.commit()



### POST방식 에 대해

<hr>

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action='/users/create', method="POST">
        USERNAME : <input type="text" name="username"> <br>
        EMAIL : <input typ="email" name="email"> <br>
        <input type="submit" value="회원가입?!"> <br>
    </form>
</body>
</html>
```

`method = "POST"`

```python
@app.route('/')
def index():
    return render_template('index.html')


@app.route('/users/new')
def new_user():
    return render_template('new.html')

@app.route('/users/create', methods=["POST"])
def create_user():
    username = request.form.get('username') #이재찬
    email = request.form.get('email') #lee@lee
    #user = User(username='이재찬',email='lee@lee')
    user = User(username=username,email=email) #class의 username,email
    db.session.add(user)
    db.session.commit()
    return render_template('create.html',username=user.username,email=user.email)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port='8080', debug=True)

```

`methods=["POST"]`를 해주면 주소창에 정보가 없어짐.



## POST

```python
# GET /users/create
# POST /users/create
```

방식은 완전히 다른 요청방식

