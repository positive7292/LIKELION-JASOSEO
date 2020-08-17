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


