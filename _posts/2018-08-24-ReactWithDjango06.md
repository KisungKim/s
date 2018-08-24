---
layout: default
title: "장고와 리액트 연동하기(Django&Django REST framework&ReactApp)"
---

> 장고에 rest framework 설치하기 

## 들어가며

<br/>
쉽게 말해 원래는 아래와 같은 순서로 장고에서 db의 query를 사용할 수 있었습니다.
<br/>

## (기존)

<br/>
1. 장고와 데이터베이스를 연결합니다(장고와 리액트 연동하기(데이터베이스(1))참고)
2. 장고 ORM (예: Something_class.objects.all())등을 통해 데이터를 받아옵니다
3. views.py에서 받아온 데이터를 장고식 문법으로 템플릿에 넘겨주고 작업합니다
<br/>
사실 여기까지가 원래 기초적인 장고에서 Model-View-Template을 통해 데이터를 만져주는 과정입니다.
<br/>
하지만 리액트를 이용할 경우 살짝 순서가 바뀝니다.

## 리액트와 장고

<br/>
1. 장고와 데이터베이스를 연결합니다
2. 장고 ORM (예: Something_class.objects.all())등을 통해 데이터를 받아옵니다
3. 받아온 데이터를 Django REST framework를 활용해 json객체로 만들어줍니다.
4. json객체를 특정 url에 뿌려줍니다.
5. React App에서 fetch를 통해 특정 url로부터 json객체를 받아옵니다.
6. 받아온 데이터를 리액트식 문법으로 가공/작업합니다
<br/>
이번 포스팅은 어떻게 장고 REST framework의 환경을 설정하는지, 그리고 REST framework를 활용한 json객체로의 serialize는 어떻게 이루어지는지에 대한 내용입니다.
<br/>
<br/>
아래는 제가 참고했던 사이트입니다. 
<div style="color:blue;font-size:20px;">
<a href="http://webframeworks.kr/tutorials/react/react-django-full-stack-spa/">React with Redux 와 Django를 이용한 SPA 개발</a>
<br/>
<a href="https://www.valentinog.com/blog/tutorial-api-django-rest-react/">Tutorial: Django REST with React (Django 2.0 and a sprinkle of testing)</a>
</div>
<br/>

# 장고 REST framework 개발환경 셋팅
<br/>
<b>첫 번째로</b> 
pip 명령어를 통해 django REST framework라이브러리를 해당 가상환경안에 설치하고, settings.py의 installed app부분에 추가해주시면 됩니다.
<br/>

```
// 저의 경우 이미 설치되어 있기 때문에 아래와 같은 메시지를 볼 수 있습니다
(venv_ver01) gimgiseong-ui-MacBook-Pro:local_testing kisung$ pip install djangorestframework
Requirement already satisfied: djangorestframework in ...(생략)...

// 지금까지 현재 가상환경 안의 설치 내용들을 다음과 같이 확인할 수 있습니다.
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ pip freeze > requirements.txt 
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ cat requirements.txt 
...(중략)...
Django==2.0.7
django-webpack-loader==0.6.0
djangorestframework==3.8.2
...(중략)...
```

이제 설치한 restframework를 settings의 installed app부분에 다음과 같이 추가해주면 됩니다.

```python

...(중략)...

# [myproject]/[project_folder]/settings.py에서 
# 아래와 같이 rest_framework를 추가해주고 migrate명령어를 통해 빌드해줍니다.

INSTALLED_APPS = [
'django.contrib.admin',
'django.contrib.auth',
'django.contrib.contenttypes',
'django.contrib.sessions',
'django.contrib.messages',
'django.contrib.staticfiles',
'app01',
'webpack_loader',
'rest_framework',
]

...(중략)...
```

# Django REST Framwork를 활용해 React app에 data 넘기기

<b>두 번째로</b> 
이제 rest_framework를 import하여 views에서 파이썬의 클래스 형식으로 넘겨받은 데이터를 json으로 serialize하는 작업이 필요합니다.
<br/>
이를 위해 새롭게 serializers.py라는 파일이 [my_app] 디렉토리 내부에 필요하며,
<br/>
<b>전체 디렉토리 구조는 [my_django_project]/[my_app]/serializers.py가 된다고 보시면 됩니다</b>
<br/>
url을 연결하여 리액트 모듈이 얹어진 template에서 해당 url의 json view를 받아들일 수 있도록 views.py와 urls.py를 수정해주셔야 합니다.
<br/>
<br/>

## serializers.py 작성하기

```python

# [myproject]/[project_folder]/serializers.py를 만들어 줍니다.
# 아래의 의미는 방금 설치한 Djago REST framework를 import하여
# serializers를 통해 본인이 만들었던 모델(예를 들어 지금 예시에서는 'Mise'라는 model class)의
# 모든 필드에 대해 serializer하는 MiseSerializer을 생성한다 의미입니다.

from rest_framework import serializers
from .models import *


class MiseSerializer(serializers.ModelSerializer):
    class Meta:
        model = Mise
        fields = '__all__'

```
## views.py 수정하기

이제 views.py로 돌아가셔서,

```python

# [myproject]/[project_folder]/views.py 입니다
# 아래의 의미는 rest framework의 generics를 활용해,
# Mise테이블의 모든 데이터를 queryset을 통해 받아와
# 받아온 데이터의 모든 필드를(serializers.py에서 모든 필드를 설정했습니다.) MiseSerializer의 클래스를 사용하여 
# json 데이터로 serialize한다는 의미입니다.

from django.shortcuts import render
from .serializers import *
from rest_framework import generics


class MiseListCreate(generics.ListCreateAPIView):
    queryset = Mise.objects.all()
    serializer_class = MiseSerializer


# Create your views here.
def test_html(requests):
    return render(requests, 'draw_map_daum.html')
```

## urls.py 수정하기

마지막으로 urls.py를 수정해 줍시다.
<br/>
아래 코드는 views.py의 MiseListCreate라는 클래스를 활용해 as_view()라는 기본 내장 function call을 통해 map_main을 render해준다는 의미입니다. 

```python
from django.urls import path
from . import views

urlpatterns = [
    path('app01/start_app', views.test_html),
    path('app01/map_main', views.MiseListCreate.as_view()),
]
```

저는 다음과 같이 두 개의 render하는 페이지를 만들었습니다.
<br/>
첫 번째 페이지의 경우(https://[host_address_with_port]/[whatever_]/app01/start_app) reactApp에서 loads된 데이터를 활용하기 위해, 
<br/>
두 번째 페이지를 통해서는(https://[host_address_with_port]/[whatever_]/app01/map_main) 기본 내장 rest framework는 데이터를 어떻게 보여주는지 알 수 있습니다.
<br/>
참고로 Mise class는 아래와 같이 models.py에서 설정해 줬습니다.

## [참고용]Mise class의 생김새

```python
# [myproject]/[project_folder]/models.py 입니다

class Mise(models.Model):
    stationname = models.CharField(max_length=100, null=True)
    datatime = models.CharField(max_length=100, null=True)
    so2value = models.CharField(max_length=10, null=True)
    covalue = models.CharField(max_length=10, null=True)
    o3value = models.CharField(max_length=10, null=True)
    no2value = models.CharField(max_length=10, null=True)
    pm10value = models.CharField(max_length=10, null=True)
    khaivalue = models.CharField(max_length=10, null=True)
    khaigrade = models.CharField(max_length=10, null=True)
    so2grade = models.CharField(max_length=10, null=True)
    cograde = models.CharField(max_length=10, null=True)
    o3grade = models.CharField(max_length=10, null=True)
    no2grade = models.CharField(max_length=10, null=True)
    pm10grade = models.CharField(max_length=10, null=True)
    lat = models.FloatField(null=True, blank=True, default=None)
    lng = models.FloatField(null=True, blank=True, default=None)
```

## React-app code

첫 번째 페이지처럼 reactApp에서 데이터를 불러오려면 다음과 같은 코드를 ComponentDidMount부분에 추가해주시면 됩니다.
<br/>
물론 수정 후 당연히 npm run-script build등 빌드 명령어를 통해 리액트 모듈을 빌드하고
<br/>
후에 python manage.py runserver을 통해 페이지를 확인하셔야 합니다
<br/>
fetch부분에 어떤 주소로 redirect해주고 있는지, 그리고 받아온 데이터를 render하는 과정에서 데이터의 undefined유무를 어떻게 확인하고 있는지에 대해 살펴보시면 될겁니다.

```

...(중략)...

constructor(props) {
    super(props);
    this.state =  {
        data: [],
        loaded: false,
        placeholder: "Loading"
    };
}

componentDidMount() {
    fetch("map_main")
        .then(response => {
            if(response.status !== 200) {
                return this.setState({placeholder: "Server Error"});
            }
            return response.json();
            })
        .then(data => this.setState({data: data, loaded: true}));
}

render() {
    const { data, loaded, placeholder } = this.state;
    if(!data[0]) {
        return (<div id="something">still waiting</div>);
    }
    else {
        return (<div id="something">{data[0].stationname}</div>);
    }
}

...(후략)...

```

두 번째 페이지처럼 불러온다면 다음과 같은 화면을 볼 수 있습니다.

![image01](https://drive.google.com/open?id=1KsHft1hJoUpUBrSbTNDjvvvT85VpO0fS)
