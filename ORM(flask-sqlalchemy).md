## ORM(flask-sqlalchemy)

1. 기본 설정

   ```python
   $ pip install flask_sqlalchemy flask_migrate
   ```

   ```python
   #app.py
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

2. flask db설정

   * 초기 설정 (`migration` 롤더 생성)

   ```python
   $ flask db init
   ```

   * migration파일 생성

   ```python
   $ flask db migrate
   ```

   * db에 반영

   ```python
   $ flask db upgrade
   ```

3. 활용법

   1. Create 

      ```python
      # user 인스턴스 생성
      users = User(username='이재찬', email='lee@gmail.com')
      # db.session.add 명령어를 통해 추가
      # INSERT INTO user (username,email)
      # VALUES ('이재찬','lee@gmail.com'); 과 동일하다.
      db.session.add(user)
      #db에 반영
      db.session.commit()
      ```

   2. READ

      ```python
      # SELECT * FROM user;
      users = User.query.all()
      # get 메소드는 primary key로 지정된 값을 통해 가져온다.
      user = User.query.get(1)
      # 특정 컬럼의 값을 가진 것을 가져오려면 다음과 같이 쓴다.
      user = User.query.filter_by(username='이재찬').all()
      user = User.query.filter_by(username='이재찬').first()
      ```

   3. UPDATE

      ```python
      user = User.query.get(1)
      user.username = '홍길동'
      db.session.commit()
      ```

   4. DELETE

      ```python
      user = User.query.get(1)
      db.session.delete(user)
      db.session.commit()
      ```


