---
layout: default
title: "리액트 네이티브 npm Module install 오류 handle하기"
---

> 리액트 네이티브 npm Module install 오류 handle하기

## 버전 : react-native 0.59.4

doc에서 명시한 바로는..
```
[name:directoryPath$ npm install --save [module_name] 
[name:directoryPath$ react-native link [module_name] 
```
가 모듈을 설치하는 방법으로 나와있다.
<br/>
하지만 리액트를 이용하며 아래와 같은 오류에 계속 부딪쳤다
```
Module [module_name] does not exist in the Haste module map
```
혹은
```
While trying to resolve module [module_name] from file `[filePath]/package.json` was successfully found. However, this package itself specifies a `main` module field that could not be resolved (`[installed_module_path]/index.js`. Indeed, none of these files exist
```
<br/>
하지만 이에 대한 명쾌한 자료가 없어 몇시간 동안 삽질하다 간단한 해결책을 찾았다.

## Terminal1

```
[name:directoryPath$ npm install --save [module_name] 
[name:directoryPath$ react-native link [module_name] 
[name:directoryPath$ rm -rf ./node_modules
[name:directoryPath$ react-native link
```
## Terminal2(running 'react-native start')

```
[name:directoryPath$ ^C                                    // 실행중인 react-native앱을 종료
[name:directoryPath$ react-native start -- --reset-cache   // restart
```
왜 이런지는 모르겠지만 적어도 모듈을 install하는 과정에서 앱이 인식 못하는 자잘한 오류들은 해결되었다.
