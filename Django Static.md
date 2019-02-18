## Django Static

### 이미지 업로드

```
mkdir static
```

home 폴더 내 static 폴더 생성

```python
path('home/static_example/',views.static_example),

def static_example(request):
    return render(request, 'static_example.html')
```

```django
{% block body %}
    {% load static %}
    <img src="{% static 'flare.jpg' %}">
{% endblock %}
```

서버 off후 다시 실행하면 사진이 뜸.



### style.css 사용하기

```django
<!--base.html-->
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>{% block title %} {% endblock %}</title>
    {% block css %}{% endblock %}
```

head에 `{% block css %}{% endblock %}` 추가

static 폴더 내 style.css 파일 생성

```css
/*style.css*/
body {
    color : red;
}
```

```django
<!--static_example.html-->
{% block css %}
<link rel="stylesheet" href="{% static 'style.css' %}" type="text/css" />
{% endblock %}

{% block body %}
    <img src="{% static 'flare.jpg' %}">
{% endblock %}
```

