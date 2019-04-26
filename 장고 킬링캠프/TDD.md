# TDD

```bash
$ pip install django_test_plus
```

<https://docs.djangoproject.com/ko/2.2/intro/tutorial05/>



### GIVEN WHEN THEN

<https://blog.outsider.ne.kr/669>

먼저 // Given 주석에서는 테스트에 필요한 객체와 준비를 갖추고 // When에서는 일을 벌인다. 주로 테스트하려는 코드를 실행한다. 그리고 마지막으로 // Then 주석 아래에서는 주로 // When에서 발생한 일의 결과 어떤 일이 벌어졌는지 확인하는 코드가 주를 이룬다. 때에 따라서는 코드를 이렇게 세 구역으로 나누기 모호하기 때문에 종종 // Given과 // When Then 둘로만 나누는 경우도 있다.



직접 서버를 실행하면서 에러를 확인하지 않아도 됨, tests.py를 통해 검증을 할 수 있어 시간절약할 수 있다.



```python
# board/tests.py
from test_plus.test import TestCase
from django.conf import settings
from .models import Board
```

```python
class SettingsTest(TestCase):
    # 1. settings 값 확인
    def test_01_settings_locale(self):
        self.assertEqual(settings.USE_I18N, True)
        self.assertEqual(settings.TIME_ZONE,'Asia/Seoul')
```

```bash
>>> OK
```

True면 Pass, False면 AssertError를 발생시킨다.



### MODEL TEST

```python
class BoardModelTest(TestCase):
    def test_01_model(self):
        board = Board.objects.create(title='테스트',content='내용',user_id=1)
        self.assertEqual(str(board),f'Board{board.pk}')
```

```bash
>>> OK
```



```python
from .forms import BoardForm
class BoardViewCreateTest(TestCase):
    # C R D U 순으로
    def test_01_get(self):
        response = self.get_check_200('boards:create')
        # self.assertContains(response, '<form')
        self.assertIsInstance(response.context['form'], BoardForm)
```

```bash
>>> AssertionError: 302 != 200
```

```python
from .forms import BoardForm
class BoardViewModelTest(TestCase):
    # C R D U 순으로
    def test_01_get(self):
        self.make_user(username='test',password='xptmxmqht!')
        with self.login(username='test',password='xptmxmqht!'):
            response = self.get_check_200('boards:create')
            # self.assertContains(response, '<form')
            self.assertIsInstance(response.context['form'], BoardForm)
```

```bash
>>> OK
```



```python
class BoardViewModelTest(TestCase):
    def test_02_modelform_with_data(self):
        data = {'title':'test', 'content': 'test'}
        self.assertEqual(BoardForm(data).is_valid(),True)
        
    def test_03_modelform_without_title(self):
        data = {'content':'test'}
        self.assertEqual(BoardForm(data).is_valid(),False)
        
    def test_03_modelform_without_content(self):
        data = {'test':'test'}
        self.assertEqual(BoardForm(data).is_valid(),False)
```

```python
# forms.py 에서 content를 지우고 실행하면 Error가 뜬다.
```



### CREATE TEST

```python
class BoardViewCreateTest(TestCase):
    # C R D U 순으로
    # given : 유저가 로그인 되었을 때.
    def setUp(self):
        user = self.make_user(username='test', password='xptmxmqht!')
        
    # when
    def test_01_get(self):
        with self.login(username='test',password='xptmxmqht!'):
            response = self.get_check_200('boards:create')
            # self.assertContains(response, '<form')
            # then
            self.assertIsInstance(response.context['form'], BoardForm)    
   
    def test_02_login_required(self):
        self.assertLoginRequired('boards:create')
        
    def test_03_post_redirect_302(self):
        # given
        data = {'title':'제목 작성함.', 'content':'냉무'}
        # when
        with self.login(username='test', password='xptmxmqht!'):
            self.post('boards:create',data=data)
            # then
            self.response_302()
        
```



### DETAIL TEST

```python
class BoardViewDetailTest(TestCase):
    def setUp(self):
        # given
        self.user = self.make_user(username='test', password='xptmxmqht!')
        self.board = Board.objects.create(
                        title='제목',
                        content='내용',
                        user=self.user
                        )
    
    # get 요청을 보내어 실제로 오는지 확인
    def test_01_get(self):
        # when then
        self.get_check_200('boards:detail',board_pk=self.board.pk)
        
    def test_02_contain_board_title_content(self):
        # when
        self.get_check_200('boards:detail',board_pk=self.board.pk)
        # self.assertResponseContains(board.title, html=False)
        # self.assertResponseContains(board.content, html=False)
        # then
        self.assertResponseContains(self.board.title)        
        self.assertResponseContains(self.board.content)
        
    def test_03_template(self):
        # when
        response = self.get_check_200('boards:detail', board_pk=self.board.pk)
        # then
        self.assertTemplateUsed(response, 'boards/detail.html')
```



### Index

```python
class BoardViewIndexTest(TestCase):
    def setUp(self):
        # given
        self.user = self.make_user(username='test', password='xptmxmqht!')
        self.board = Board.objects.create(
                                title='제목',
                                content='내용',
                                user=self.user
                                )
                                
    def test_01_boards_queryset(self):
        # given
        Board.objects.create(
                        title='제목1',
                        content='내용1',
                        user=self.user
                        )
        boards = Board.objects.order_by('-pk')
        # when
        response = self.get_check_200('boards:index')
        # then
        self.assertQuerysetEqual(response.context['boards'], map(repr, boards))
    
    def test_02_template(self):
        # when
        response = self.get_check_200('boards:index')
        # then
        self.assertTemplateUsed(response, 'boards/index.html')
```



### DELETE TEST

```python
class BoardViewDeleteTest(TestCase):
    def setUp(self):
        # given
        self.user = self.make_user(username='test', password='xptmxmqht!')
        self.board = Board.objects.create(
                                title='제목',
                                content='내용',
                                user=self.user
                                )
                                
    def test_01_delete_get(self):
        with self.login(username='test',password='xptmxmqht!'):
            self.get('boards:delete',board_pk=self.board.pk)
            self.response_405()
            
    def test_02_post(self):
        with self.login(username='test',password='xptmxmqht!'):
            self.post('boards:delete',board_pk=self.board.pk)
            self.response_302()
            
    def test_03_delete(self):
        with self.login(username='test',password='xptmxmqht!'):
            self.post('boards:delete',board_pk=self.board.pk)
            self.assertEqual(Board.objects.all().count(), 0)
```



### UPDATE TEST

```python
class BoardViewUpdateTest(TestCase):
    def setUp(self):
        # given
        self.user = self.make_user(username='test', password='xptmxmqht!')
        self.board = Board.objects.create(
                                title='제목',
                                content='내용',
                                user=self.user
                                )
    def test_01_boardform_instance(self):
        with self.login(username='test',password='xptmxmqht!'):
            response = self.get_check_200('boards:update',board_pk=self.board.pk)
            self.assertEqual(response.context['form'].instance.pk, self.board.pk)
```





### 에러

* 302 != 200

  302는 redirect 로그인 관련 에러. 로그인을 해줘야한다.

* 405 != 200

  405는 허용되지 않은 method 사용

