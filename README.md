# 2강

## model & database

>> 장고와 데이터베이스는 별도의 공간에 있음

>> 장고와 데이터베이스의 상호작용은 ORM이라는 방식을 활용

>> Model은 데이터의 틀, 저장공간 

>> 모델을 짜고 데이터베이스에게 알려주면 그 틀에 맞추어서 실행됨.

>> 모델의 각각의 독립적인 데이터는 오브젝트라고 함.

>> 장고 프로젝트와 데이터 베이스의 상호작용

* 데이터베이스에게 모델의 변경 사항을 알려주는 방법

>> python manage.py makemigrations와 python manage.py migrate로 알려줌

* 가상환경 끄는 법
>> deactivate

### 모델 만들기
* models.fy

~~~
class Jasoseol(models.Model):
    title = models.CharField(max_Length=50)
    content = models.TextField()
    updated_at = models.DateTimeField(auto_now=True)
~~~

#### 다양한 필드들
>> primary key : AutoField

>> 문자열 : CharField, TextField, SlugField

>> 숫자 : IntegerField, PositiveIntegerField, FloatField

>> 날짜/시간 : DataField, TimeField, DataTimeField

>> 참/거짓 : BooleanField, NullBooleanField

>> 파일 : FileField, ImageField, FilePathField

#### admin, 관리자 계정 만들기
* python manage.py createsuperuser

* admin.py

>> from .models import Jasoseol

>> admin.site.register()

# 3강

## 모델폼

>> 모델에 대응하는 html폼을 만들어 줌

>> 데이터를 생성하거나 업데이트가 간편

>> 폼을 다루는 법을 배워야함

* display flex
>> p 태그나 다른 요소들을 컨트롤할 수 있음

* forms.py

~~~
from django import forms
from .models import Jasoseol

class JssForm(forms.ModelForm):

    class meta:
        model = Jasoseol
        fields = ('title', 'content',)
~~~

# 4강

## primary key

>> 중복될 수 없는 단일 값

>> 오브젝트를 식별할 수 있는 값

>> 만약 pk를 설정하지 않는다면 id가 만들어짐

>> detail.html -- views.py -- urls.py 

* pk 설정하는 법
~~~
class MyModel(models.Model):
        my_pk = models.IntegerField(primary_key=True)
~~~

1. detail 생성
~~~
def detail(request, jss_id):
    # my_jss = jasoseol.objects.get(pk=jss_id)
    my_jss = get_object_or_404(jasoseol, pk=jss_id)
    return render(request, 'detail.html', {'my_jss':my_jss}
~~~

2. delete 생성
~~~
def delete(request, jss_id):
    my_jss = jasoseol.objects.get(pk=jss_id)
    my_jss.delete()
    return redirect('index')
~~~
3. update 생성
~~~
def update(request, jss_id):
    my_jss = jasoseol.objects.get(pk=jss_id)
    jss_form = JssForm(instance=my_jss)
    if request.method == "POST":
      updated_form = JssForm(request.POST, instance=my_jss)
      if updated_form.is_valid():
          updated_form.save()
          return redirect('index')
~~~

# 5,6강

## USER 모델

>> Django에서 지원하는 시스템

## URL 상속

>>  urls.py

~~~
from django.urls import path
urls.py -> path('', include('main.urls'))
import include
~~~

## 회원가입

>> views.py

~~~
from django.contrib.auth.forms import UserCreationForm

def signup(request):

    regi_form = UserCreationForm()
    if request.method == "POST":
        filled_form = UserCreationForm(request.POST)
        if filled_form.is_valid():
            filled_form.save()
            return redirect('index')
            
        # else:
        

    return render(request, 'signup.html', {'regi_form':regi_form})
~~~

## 로그인, 로그아웃

>> urls.py

~~~
from django.contrib.auth.views import LoginView, LogoutView

urlpatterns = [
    path('signup/', signup, name='signup'),
    path('login/', LoginView.as_view(), name='login'),
    path('logout/', LogoutView.as_view(), name='logout'),
]
~~~

# 7강

## Foreign key

>> 데이터의 참조 무결성을 확인하기 위하여 사용

>> maim.models.py

~~~
class Jasoseol(models.Model):
    title = models.CharField(max_length=50)
    content = models.TextField()
    updated_at = models.DateTimeField(auto_now=True)
    author = models.ForeignKey(User, on_delete=models.CASCADE)
~~~

# 8강

## 댓글 만들기

>> views.py

~~~
def create_comment(request, jss_id):
    comment_form = CommentForm(request.POST)
    if comment_form.is_valid():
        temp_form = comment_form.save(commit=False)
        temp_form.author = request.user
        temp_form.jasoseol = Jasoseol.objects.get(pk=jss_id)
        temp_form.save()
        return redirect('detail', jss_id)

def delete_comment(request, jss_id, comment_id):
    my_comment = Comment.objects.get(pk=comment_id)
    if request.user == my_comment.author:
        my_comment.delete()
        return redirect('detail', jss_id)

    else:
        raise PermimssionDenied
~~~

# 9강

## 글자수 세기

>> 요소 선택 -- 이벤트 핸들러

>>> 요소 선택 : querySelector

>>> 이벤트 핸들러 : 요소.addEventListener

* count.js

>> 요소 선택

~~~
const targetForm = document.querySelector('.jss_content_form')
const counted_text = document.querySelector('.counted_text')
~~~

>> 이벤트 핸들러

~~~
targetForm.addEventListener("keyup", function() {
    counted_text.innerHTML = targetForm.value.length
})
~~~




