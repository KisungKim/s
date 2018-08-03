---
layout: default
title: "StandAlone파일과 장고(1)"
---

> 매우 기초적인 장고와 StandAlone 파일 연동하기

## 연동 시작하기 : 장고 envir과 연동

아래 docs를 보시면 자세한 내용을 확인할 수 있습니다.<br/>
<a href="https://docs.djangoproject.com/en/2.1/topics/settings/#calling-django-setup-is-required-for-standalone-django-usage">calling-django-setup-is-required-for-standalone-django-usage</a><br/>

<br/>
장고를 통해 작업을 하다 보면 
<br/>
1. 공동작업환경에서 해당project의 환경으로 setting하기
2. standalone파일이 있을 경우 장고와 연동시키기(예: 크롤링 데이터 받아오기 등)
<br/>
가 있을 수 있습니다.
<br/>
이번 포스팅은 두 번째 내용에 관련된 내용입니다.

<a href="https://github.com/YeongBaeeee/practice/wiki/%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0">
https://github.com/YeongBaeeee/practice/wiki/배포하기</a>

<br/>
<b>첫 번째로</b> import os를 통해 현재 파일에서 something django project의 환경을 사용하겠다고 명시를 해주셔야 합니다. standalone파일(예를 들어 다음 뉴스를 크롤링하는..)은 작업중인 venv안에서 실행되었다 하더라도 장고프로젝트와는 독립된 파일이라고 볼 수 있습니다. 따라서 os에게 '이 파일은 (장고의)somethingproject 위에서 돌아가는 파일이야'라는 식의 언급이 필요하며 os.environ.setdefault("DJANGO_SETTINGS_MODULE", 'someproject.settings') 가 그 방법입니다.
<br/>
<br/>
<b>두 번째로</b> import django를 통해 위에서 명시한 장고환경을 파일 안에서 setup해주는 코드가 필요합니다. '우린 이런 환경을 쓸거야'라고 명시를 해주었으니 이제 그 파일에서 그 환경을 쓰겠다는 말과 같습니다.
<br/>
(이때 당연히 코드의 순서는 중요합니다)

# 방법예시(데이터를 장고 db에 insert하려면?)

```python

# 이건 내가 사용하려는 환경이야(순서1)
import os
os.environ.setdefault("DJANGO_SETTINGS_MODULE", 'local_testing.settings')
# 자 이제 셋업해줘(순서2)
import django
django.setup()

# 이제 우리는 ~~를 작업할거야(순서3)
from [프로젝트 어플리케이션].models import [사용하려는 클래스(table)]

# 작업할 때 필요한 모듈(아래는 그냥 예시입니다)
import requests
from bs4 import BeautifulSoup

# 본인의 코드(아래는 그냥 예시입니다)
# requests를 통해 해당 url에서 데이터를 불러오고
# BeautifulSoup을 통해 데이터를 정제해서
# [사용하려는 클래스(table)]에 bulk_create하겠습니다
# 라는 코드가 들어올 수 있습니다
```

