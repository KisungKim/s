## 연동 시작하기 : 장고 url과 연동

장고걸스를 통해 내용을 확인해보실 수 있습니다.<br/>
이제부터 건드려야 할 파일은 크게 5가지가 있습니다<br/>

1. [Project]/[Application]/static/js/index.js를 작성해 줍니다
2. [Project]/[Application]/templates/testhtml01.html (이때 templates 디렉토리를 생성해주셔야 합니다)
3. [Project]/[Application]/views.py
4. [Project]/[Application]/urls.py
5. [Project]/[Project]/urls.py
6. [Project]/[Project]/settings.py

순서는 차례로 6 -> 1로 올라가시는게 이해에 도움이 됩니다<br/>

# [Project]/[Project]/settings.py

```python

...(중략)...

# Installed Apps에 위에서 설치한 webpack_loader를 추가해 줍니다
INSTALLED_APPS = [
'django.contrib.admin',
'django.contrib.auth',
'django.contrib.contenttypes',
'django.contrib.sessions',
'django.contrib.messages',
'django.contrib.staticfiles',
'app01',
'webpack_loader',

]

# Webpack Loader(설치한 장고-웹팩로더의 환경설정입니다 아래의 코드를 추가하셔야 합니다)
WEBPACK_LOADER = {
'DEFAULT': {
'BUNDLE_DIR_NAME': 'bundles/',
'STATS_FILE': os.path.join(BASE_DIR, 'webpack-stats.json'),
}
}

...(중략)...

```

# [Project]/[Project]/urls.py

```python

from django.contrib import admin
from django.urls import path, include

# path('', include('app01.urls'))의 의미는 
# localhost:portnum/으로 들어온 클라이언트를
# app01폴더 내부의 urls.py에 명시된 주소로 redirect해줌을 의미합니다.
urlpatterns = [
path('admin/', admin.site.urls),
path('', include('app01.urls'))
]

```

# [Project]/[Application]/urls.py

```python

from django.urls import path
from . import views

# 아래 코드를 추가함으로써 이제 사용자는
# localhost:portnum/app01/test01을 통해 views파일의 test01이라는 함수를 작동시키게 됩니다
urlpatterns = [
path('app01/test01', views.test01)
]
```

# [Project]/[Application]/views.py

```python

from django.shortcuts import render


# Create your views here.
# 이제 test01 함수가 requests를 받아 실행되며 testhtml01.html파일을 렌더링해주게 됩니다
def test01(requests):
return render(requests, 'testhtml01.html', {})


```

# [Project]/[Application]/templates/testhtml01.html
포스팅상 문제때문에 코드를 분리하였습니다
<br/><a href="https://medium.com/uva-mobile-devhub/set-up-react-in-your-django-project-with-webpack-4fe1f8455396">링크에서</a><br/> 
'In your templates folder, add the following to a new or existing html file:'의 내용
<br/>아래에 있는 코드를 붙여넣으면 됩니다
<br/>혹시나 문제가 발생할 경우 첫 번째 라인과 두 번쨰 라인의 순서를 바꿔주세요

#  [Project]/[Application]/static/js/index.js를 작성해 줍니다

앞으로 번들의 시작점이 되는 index.js 파일은 다음과 같습니다.

```
import React from 'react'
import ReactDOM from 'react-dom'


function Welcome(props) {
return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="world" />;
ReactDOM.render(
element,
document.getElementById('react')
);
```

이제 콘솔창에서 다음과 같이 서버를 실행시키면 성공적으로 hello world를 확인할 수 있습니다<br/>
주소는 예를 들어 'http://localhost:8000/app01/test01' 라고 볼 수 있습니다<br/>

```
// 콘솔명령이 실행되는 디렉토리를 확인하세요
(venv_ver01) gimgiseong-ui-MacBook-Pro:local_testing kisung$ python manage.py runserver

```
