```python
boards = Board.objects.all()

{% for board in boards %}

	{{ board.comment_set.all.count }}

{% endfor %}
```

> 데이터가 board만큼 중복??으로 출력된다고 하는데 . 이를 방지하기 위해 prefetch_related기능을 사용한다.

```python
boards = Board.objects.prefetch_related('comment_set').all()

{% for board in boards %}

	{{ board.comment_set.all.count }}

{% endfor %}
```



* annotate : 컬럼을 추가해서 값을 넣어놓는 것.



* prefetch_related : 1:N/M:N에서 N개를 미리 가져올 때 (JOIN table)

  * User.prefetch_related('score_set').get(pk=1)

    

* select_related : 1:N에서 1개인 녀석을 미리 가져 올 때 (JOIN)



```python
# views.py
from movies.models import Score
from django.db.models import Prefetch, Count

def detail(request, user_pk):
    User = get_user_model()
    user = get_object_or_404(User.objects.annotate(
                                followers_count=Count('followers'),
                                followings_count=Count('followings')
                                ), pk=user_pk)
    # user = User.objects.get(pk=user_pk)
    followings = user.followings.prefetch_related(
                                            Prefetch('score_set',
                                            queryset=Score.objects.order_by('-value'),
                                            to_attr='score_set_value_order'
                                            ))
    context = {'user_detail':user,'followings':followings}
    return render(request, 'accounts/detail.html',context)
```

```html
<!--detail.html-->
<p> 팔로잉 : {{ user_info.followings_count }}</p>
<p> 팔로워 : {{ user_info.followers_count }}</p>

<p>내 친구가 좋아하는 영화</p>
{% for user_follow in followings %}
    <p>{{ user_follow.score_set_value_order.0.movie.title }}</p>
    <p>{{ user_follow.score_set_value_order.0.value }}</p>
{% endfor %}
```



