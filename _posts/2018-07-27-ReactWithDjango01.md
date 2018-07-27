---
layout: default
title: "장고와 리액트 연동하기(1)"
---

> 매우 기초적인 장고와리액트 개념+시작하기
>

## 장고는 어떤 것이고 리액트는 어떤 것일까

<section style="color:gray">
<h6>"이번 포스팅은 제가 4학년 1학기 여름방학 때, 친구와 프로젝트를 수행하며 프론트 라이브러리인 리액트와<br/>
백엔드 프레임워크인 장고를 연동하는 과정에서 배웠던 점을 공유하는데에 목적이 있습니다"
</h6>
</section>
<br/>
<br/>
<section>
프로젝트를 시작하는 맨 처음, 나는 js를 비롯한 프론트를 거의 접해보지 못했고 친구는학기중에 react를 조금 접한 수준.<br/> 
따라서 앞으로 기획이나 프로젝트를 완성시키기 위해서 내가 그나마 접해보았던 장고와 친구가 활용 가능한 ReactApp을 연동시키기 위해 구글링을 해보았다.
<br/>
<br/>
<span style="background-color:yellow"><b>리엑트?자바스크립트?가 거의 처음이라면, 시간이 많다면 옆 튜토리얼을 직접 따라해보면 됩니다>> 
</b></span>
<a href="https://reactjs.org/tutorial/tutorial.html"> 리액트 튜토리얼</a>
<br/>
<span style="background-color:yellow"><b>
장고가 처음이라면, 옆 튜터리얼을 직접 따라해보면 됩니다>> 
</b></span>
<a href="https://tutorial.djangogirls.org/ko/django/"> 장고걸스</a>
<br/>
<br/>
기타 개념에 대한 내용은 구글에 친절한 분이 많으니 검색해서 같은 글 몇번 보다 보면 이해가 될겁니다
</section>

## 시작하기: Python3, MacOs, Django2.0을 사용했습니다
1. 먼저 프로젝트 폴더를 만들고 장고 프로젝트를 생성해야 합니다.<br/>
2. 장고 프로젝트를 생성하기 전, 가상환경을 먼저 셋팅해 줍니다<br/>
가상환경은 (다른 여러 이유가 있겠지만) 독립적인 개발환경셋팅과 더불어
앞으로의 개발에서 타인과 api들을 자유롭게 공유할 수 있는데 도움을 줍니다<br/>
<h6 style="color:gray">
"예를 들어 본인이 재수학원에서 재수공부를 한다고 가정해봅시다. 물론 재수공부를 카페, 공공도서관, 집 등에서 할 수도 있겠지만
재수학원과 가까운 독서실에서 공부하면 더 효율이 오를겁니다. 이때 '재수학원과 가까운 독서실'은 평소에 친구를 만나거나 부모님을
만나는 장소와는 독립된 장소인데, 가상환경도 이와 비슷하다고 생각하면 됩니다. 본인이 컴퓨터에서 평소에 개발하던 환경과 독립된 하나의
환경을 만든다고 할 수 있겠죠. 만약 그 독서실에 본인 자리가 있다면,
본인이 공부하던 책들도 그곳에 있을테니 다른곳에서 공부할 때에는 그 안의 책들만 가져와 장소만 바꾸면 됩니다. '책들만 가져와 장소만 바꾼다'
는 개발과정에서 내가 설치한 api의 목록들을 타인의 개발환경에 쉽게 적용시켜 설치할 수 있음을 의미합니다"
</h6>

```
// 파이썬 명령어를 통해 venv_ver01이라는 가상환경을 만듭니다
gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ python -m venv venv_ver01
gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ ls
venv_ver01

// 이제 생성된 venv_ver01이라는 폴더에서 bin 내부에 activate파일을 source라는 명령어로 실행시켜 가상환경안으로 들어갑니다
gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ source venv_ver01/bin/activate

// 아래는 가상환경에 들어온 상태입니다
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ 

// 이제 장고 프레임워크를 설치해 줍니다, django~=[숫자] 에서 숫자는 본인이 설치하려는 장고의 버젼을 의미합니다. 파이썬 버젼에따라 호환장고 버젼도 다르니 참고하세요 
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ pip install django~=2.0
Collecting django~=2.0
  Using cached https://files.pythonhosted.org/packages/ab/15/cfde97943f0db45e4f999c60b696fbb4df59e82bbccc686770f4e44c9094/Django-2.0.7-py3-none-any.whl
Collecting pytz (from django~=2.0)
  Using cached https://files.pythonhosted.org/packages/30/4e/27c34b62430286c6d59177a0842ed90dc789ce5d1ed740887653b898779a/pytz-2018.5-py2.py3-none-any.whl
Installing collected packages: pytz, django
Successfully installed django-2.0.7 pytz-2018.5

// 아까 내가 설치한 api의 목록들을 볼 수 있다고 했습니다. 그 목록들은 아래의 명령어를 통해 파일을 만들어 확인 가능합니다.
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ pip freeze > requirements.txt
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ cat requirements.txt
Django==2.0.7
pytz==2018.5
// 위에 보시면 방금 설치한 Django 2.0.7 버젼이 설치된 것을 볼 수 있습니다(pytz는 무슨놈일까..)

// 이제 장고 프로젝트를 만들어 줍니다. django-admin이라는 명령어에 startproject옵션으로 local_testing이라는 이름의 프로젝트를 생성했습니다
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ django-admin startproject local_testing
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ ls
local_testing		requirements.txt	venv_ver01

// 프로젝트 내부로 들어갑니다
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ cd local_testing/
(venv_ver01) gimgiseong-ui-MacBook-Pro:local_testing kisung$ ls
local_testing	manage.py

// manage.py 파일에 startapp옵션을 통해 app01이라는 이름의 앱을 만들어 줍니다
// 하나의 프로젝트에는 여러 앱이 있을 수 있습니다
(venv_ver01) gimgiseong-ui-MacBook-Pro:local_testing kisung$ python manage.py startapp app01
(venv_ver01) gimgiseong-ui-MacBook-Pro:local_testing kisung$ ls
app01		local_testing	manage.py
(venv_ver01) gimgiseong-ui-MacBook-Pro:local_testing kisung$ cd app01/
(venv_ver01) gimgiseong-ui-MacBook-Pro:app01 kisung$ ls
__init__.py	admin.py	apps.py		migrations	models.py	tests.py	views.py

// 생성된 app01폴더 내부로 들어가 urls.py를 만들어 줍니다
// urls.py는 앞으로 많이 활용될 것입니다. 
// 예를 들어서 localhost:portnumber/local_testing/app01/page1 과
//             localhost:portnumber/local_testing/app01/page2 등으로 client를 redirect해줄 때 앞으로 사용됩니다
(venv_ver01) gimgiseong-ui-MacBook-Pro:app01 kisung$ vim urls.py
(venv_ver01) gimgiseong-ui-MacBook-Pro:app01 kisung$ ls
__init__.py	admin.py	apps.py		migrations	models.py	tests.py	urls.py		views.py
```

<section>
이제 migrate라는 명령어가 있습니다 /[project명]/settings.py 의 내용을 build한다고 보시면 되는데 그 전에 settings.py를 좀 건드려 줍니다.
</section>

```python
# 먼저 Allowedhost 부분과(이 부분은 여러분이 페이지를 확인하며 접속할 때 허용하는 주소를 포함합니다 '*' 은 모든 접근에 대해 허용한다는 의미)
# 일단은 아래와 같이 작성합니다
ALLOWED_HOSTS = ['127.0.0.1', '.pythonanywhere.com', 'localhost']

# 두 번째로 설치한 app01을 root프로젝트에서 인식할 수 있도록 아래와 같이 적어줍니다.
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app01',
]

# 데이터베이스의 경우 지금은 활용하지 않을 예정입니다. 구글링하시면 mysql등 본인의 db와 연동시킬 수 있는 방법을 찾을 수 있습니다
# 나머지 내용은 '장고걸스'를 참고하시면 됩니다
```

이제 디렉토리 위치를 이동해 build합니다

```
(venv_ver01) gimgiseong-ui-MacBook-Pro:app01 kisung$ cd .. 
(venv_ver01) gimgiseong-ui-MacBook-Pro:local_testing kisung$ python manage.py makemigrations
No changes detected
(venv_ver01) gimgiseong-ui-MacBook-Pro:local_testing kisung$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying sessions.0001_initial... OK
(venv_ver01) gimgiseong-ui-MacBook-Pro:local_testing kisung$
```

이제 리엑트 라이브러리를 연결시키기 위한 준비가 끝났습니다. 다음은 리엑트 라이브러리를 어떻게 연결시킬 지 적을 예정입니다.
