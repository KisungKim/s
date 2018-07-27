---
layout: default
title: "장고와 리액트 연동하기(2)"
---

> 매우 기초적인 장고와리액트 연동하기

<section style="color:gray">
<h6>"이번 포스팅은 제가 4학년 1학기 여름방학 때, 친구와 프로젝트를 수행하며 프론트 라이브러리인 리액트와<br/>
백엔드 프레임워크인 장고를 연동하는 과정에서 배웠던 점을 공유하는데에 목적이 있습니다"
</h6>
<br/>
<br/>
<span style="background-color:yellow"><b> 제가 처음 따라해본 연동 튜토리얼입니다>> 
</b></span>
<a href="https://www.valentinog.com/blog/tutorial-api-django-rest-react/"> 리액트 튜토리얼(1)</a>
<br/>
<span style="background-color:green;color:white;"><b>
본 포스팅의 기반이 되는 튜토리얼입니다>> 
</b></span>
<a href="https://medium.com/uva-mobile-devhub/set-up-react-in-your-django-project-with-webpack-4fe1f8455396"> 리액트 튜토리얼(2)</a>
<br/>
<br/>
기타 개념에 대한 내용은 구글에 친절한 분이 많으니 검색해서 같은 글 몇번 보다 보면 이해가 될겁니다<br/>
첫 번째 튜토리얼은 친절합니다. 왜 웹팩을 사용하는지 등 설명을 볼 수 있습니다<br/>
그럼에도 불구하고 두 번째 튜토리얼을 따라한 이유는 앞으로 프로젝트에서 좀 더 raw하게,<br/>
본인에 입맞에 맞추어 웹팩을 개발하는데에 도움이 될 것 같아서입니다<br/>
거꾸로 말하면 첫 번째 튜토리얼의 경우 작성하신 분의 custom환경이 강하게 들어있는 느낌을 받았습니다.<br/>
</section>

## 연동 시작하기 : 필요한 모듈 설치 & 환경설정

간단히 말해 '웹팩'과 '바벨', 그리고 '리액트'가 필요합니다<br/>


1. 바벨은 비 js파일들을 js파일들로 로드해주는 로더,
2. 웹팩은 바벨로더를 통해 로드된, 앞으로 개발하면서 수많은 js파일들을 하나의 모듈로 묶어주는 모듈러 입니다.


자세한 내용은 아래의 글을 참고하면 확인하실 수 있습니다<br/>
<a href="http://blog.jeonghwan.net/js/2017/05/15/webpack.html">웹팩의 기본 개념</a><br/>
npm을 통해 설치하기 전에 먼저 npm을 초기화해줍니다.<br/>

```
// 현재 디렉토리를 확인하세요.. 진행중인 프로젝트의 root 디렉토리 입니다
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ ls
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ npm init

// 설치가 끝나면 다음과 같은 파일 목록들이 생성된 것을 볼 수 있습니다
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ ls
local_testing        package.json        requirements.txt    venv_ver01
```

앞으로 package.json은 다른 개발자 입장에서 이 프로젝트에 어떤 개발api가 사용되었고,
npm watch, build의 명령어가 어떻게 수행되는지를 보여주는 중요한 파일입니다.
npm을 통해 위에서 필요한 api들을 설치할 것입니다. 그 명령어는 다음과 같습니다
<ul>
<li>npm install [SomethingApi] : SomethingApi 를 설치합니다(install 의 경우 i로 축약이 가능합니다)</li>
<li>npm uninstall [SomethingApi] : SomethingApi 를 현재 개발환경에서 제거합니다</li>
</ul>

```
// 먼저 package.json을 열어 다음의 내용을 추가해줍니다
---/package.json---
...(생략)...
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1",
"build": "webpack --config webpack.config.js --progress --colors --mode development",
"watch": "webpack --config webpack.config.js --watch --mode development"
},
...(생략)...
--------------------

// 이후 콘솔창으로 돌아옵니다
// 바벨을 설치합니다
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ npm i babel-core babel-loader babel-preset-env babel-preset-react --save-dev

// 웹팩을 설치합니다
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ npm i webpack webpack-bundle-tracker webpack-cli --save-dev

// 리액트를 설치합니다
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ npm install react react-dom --save

// 보이는 --save와 --save-dev의 차이점은 구글 검색을 통해 바로 확인하실 수 있습니다.
// 이제 본인이 설치한 개발api환경을 cat명령어를 통해 package.json파일을 읽어 확인해봅니다.
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ cat package.json
{
"name": "testreact",
"version": "1.0.0",
"description": "",
"main": "index.js",
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1",
"build": "webpack --config webpack.config.js --progress --colors --mode development",
"watch": "webpack --config webpack.config.js --watch --mode development"
},
"author": "",
"license": "ISC",
"devDependencies": {
"babel-core": "^6.26.3",
"babel-loader": "^7.1.5",
"babel-preset-env": "^1.7.0",
"babel-preset-react": "^6.24.1",
"webpack": "^4.16.2",
"webpack-bundle-tracker": "^0.3.0",
"webpack-cli": "^3.1.0"
},
"dependencies": {
"react": "^16.4.1",
"react-dom": "^16.4.1"
}
}

// 이제 간단한 환경설정이 필요합니다. touch 명령어를 통해 일단 빈 파일을 만들어 줍니다.
// mkdir -p 옵션은 디렉토리를 한번에 하나 이상 추가할 때 사용 가능합니다. 구글을 참고하세요
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ mkdir -p local_testing/app01/static/js
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ touch local_testing/app01/static/js/index.js
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ touch webpack.config.js
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ touch .babelrc
```

webpack.config.js에는 다음과 같은 내용이 들어갑니다

```
var path = require("path");
var webpack = require('webpack');
var BundleTracker = require('webpack-bundle-tracker');

module.exports = {
context: __dirname,

// build 시작점
// 예시 ./[projectName]/[ApplicationName]/static/js/index
entry: './local_testing/app01/static/js/index',

// build의 결과
// 예시 ./[projectName]/[ApplicationName]/static/bundles
output: {
path: path.resolve('./local_testing/app01/static/bundles/'),
filename: "[name]-[hash].js",
},

// 예시 ./[projectName]/webpack-stats.json
plugins: [
new BundleTracker({filename: './local_testing/webpack-stats.json'}),
],
module: {
rules: [
{
test: /\.js$/,
exclude: /node_modules/,
use: ['babel-loader']
}
]
},
resolve: {
extensions: ['*', '.js', '.jsx']
}

};
```

.babelrc에는 다음과 같은 내용이 들어갑니다

```
{
"presets": [
"babel-preset-env",
"react"
]
}
```

이제 위의 내용들을 다음과 같이 콘솔창에서 빌드해줍니다

```
// 프로젝트의 root디렉토리 내에서 ./node_modules/.bin/webpack 파일을 통해 --config 옵션에 방금 작성한 webpack.config.js를 실행시킵니다
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ ./node_modules/.bin/webpack --config webpack.config.js
(node:25705) DeprecationWarning: Tapable.plugin is deprecated. Use new API on `.hooks` instead
Hash: 1bba607731b0aa0e4286
Version: webpack 4.16.2
Time: 2316ms
Built at: 2018. 07. 27. 오후 7:10:12
Asset     Size  Chunks             Chunk Names
main-1bba607731b0aa0e4286.js  100 KiB       0  [emitted]  main
Entrypoint main = main-1bba607731b0aa0e4286.js
[5] ./local_testing/app01/static/js/index.js 549 bytes {0} [built]
+ 14 hidden modules

WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/
// 이제 빌드결과를 보기 위해 npm 명령어로 run watch를 실행합니다
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ npm run watch

> testreact@1.0.0 watch /Users/kisung/network_skku/workspace/basicDjangowithReact_ver2
> webpack --config webpack.config.js --watch --mode development


webpack is watching the files…

(node:25708) DeprecationWarning: Tapable.plugin is deprecated. Use new API on `.hooks` instead
Hash: b5daa1da876d43a985ce
Version: webpack 4.16.2
Time: 728ms
Built at: 2018. 07. 27. 오후 7:10:23
Asset     Size  Chunks             Chunk Names
main-b5daa1da876d43a985ce.js  713 KiB    main  [emitted]  main
Entrypoint main = main-b5daa1da876d43a985ce.js
[./local_testing/app01/static/js/index.js] 549 bytes {main} [built]
+ 21 hidden modules

// 위를 보시면 main-b5daa~~~.js파일을 통해 index.js로부터 생성된 모듈 결과물을 확인하실 수 있습니다
// Ctrl C를 통해 빠져나옵니다
```

이제 장고 requirements에서 마지막으로 필요한 모듈을 추가합니다

```
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ vim requirements.txt 

//아래의 django-webpack-loader을 적어주고 저장합니다
---/requirements.txt---
django-webpack-loader
...(생략)...
--------------------

// install -r은 requirements.txt에 명시된 api들을 자동으로 설치해주겠다는 의미입니다
// 결과적으로 django-webpack-loader가 설치됩니다.
(venv_ver01) gimgiseong-ui-MacBook-Pro:basicDjangowithReact_ver2 kisung$ pip install -r requirements.txt 
Collecting django-webpack-loader (from -r requirements.txt (line 1))
Downloading https://files.pythonhosted.org/packages/59/95/5f22937ebb7aae30bbd8e1d4f428a36bdd98ac97c3ed12ebba9c6902496c/django_webpack_loader-0.6.0-py2.py3-none-any.whl
Requirement already satisfied: Django==2.0.7 in ./venv_ver01/lib/python3.6/site-packages (from -r requirements.txt (line 2)) (2.0.7)
Requirement already satisfied: pytz==2018.5 in ./venv_ver01/lib/python3.6/site-packages (from -r requirements.txt (line 3)) (2018.5)
Installing collected packages: django-webpack-loader
Successfully installed django-webpack-loader-0.6.0
```

## 연동 시작하기 : 장고 url과 연동

장고걸스를 통해 내용을 확인해보실 수 있습니다.<br/>
이제부터 건드려야 할 파일은 크게 5가지가 있습니다<br/>

0. [Project]/[Application]/static/js/index.js를 작성해 줍니다
1. [Project]/[Application]/templates/[html파일] (이때 templates 디렉토리를 생성해주셔야 합니다)
2. [Project]/[Application]/views.py
3. [Project]/[Application]/urls.py
4. [Project]/[Project]/urls.py
5. [Project]/[Project]/settings.py

순서는 차례로 5 -> 1로 올라가시는게 이해에 도움이 됩니다<br/>

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

# [Project]/[Application]/templates/[html파일] 

```
<!DOCTYPE html>
{% load render_bundle from webpack_loader %}
<html>
<head>
<meta charset="UTF-8">
<title>Example</title>
</head>
<body>
<div id="react"></div>
{% render_bundle 'main' %}
</body>
</html>
```

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
