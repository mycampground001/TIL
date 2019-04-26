1. 사용자 정보를 받을 수 있도록 한다.

   Form을 줘야하는데 예전에는 <form>을 직접 썼지만 현재는 모델폼을 쓰고 있다. 그래서 GET 요청으로 들어오면 나는 모델폼을 context에 담아서 준다.

2. 사용자가 정보를 보내준다.

   POST요청이므로 request.POST에 사용자 정보가 담겨있다. 이를 모델폼에 넘겨주고 검증을 하게된다. (is_valid) 저장단계에서 정보가 더 필요하면 commit = False로 저장을 멈춰놓고 해당하는 값을 넣고 .save() 한 뒤 넘겨준다.



회원가입/로그인

1. GET

   ModelForm context로 보내기

   지정된 템플릿에 출력하기

   POST 요청은 csrf_token

2. POST

   기존코드는 GET만 처리하니까 request.method를 통해 분기

   ModelForm으로 데이터 받기(request.POST)

   데이터 검증하기

   ​	통과하면 저장

   ​	저장 전 정보 더 추가해야하면(user) commit=False

   ​	통과 못하면 처리 x

   