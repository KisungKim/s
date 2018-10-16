---
layout: default
title: "리액트와 다음지도(ReactApp&Daum API)"
---

> 리액트를 활용해 다음 지도 api 활용하기 

## Overview

<br/>
아래의 기능을 리액트를 활용해 구현하였습니다
<br/>
다음 지도 api의 sample에는 html, js 코드밖에 없기에 리액트 코드로 구현한 기능을 공유합니다
<br/>
다음지도를 활용하여 웹어플리케이션의 구현하실 때 참고하시면 됩니다

1. 검색창을 활용해 지도 실행 및 이동시키기
2. 현재 지도의 중심좌표를 기준으로 행정동 주소와 위도 경도를 받아오기
3. 드래그 이벤트 발생 시, 활용방법 예시
4. 지도의 크기를 조정하는 방법 예시
5. 현재 지도를 static이미지로 받아오기

<div style="color:blue;font-size:30px">
<br/>
더 자세한 내용은 다음docs에 자세히 기술되어 있습니다
<a href="http://apis.map.daum.net/"> 다음 지도 api 활용하기</a>
<br/>
아래 깃허브 주소에서 실제로 구현한 코드를 참고하세요
<a href="https://github.com/KisungKim/ReactWithDaumMap"> 구현한 코드 보기</a>
</div>

## 들어가며

> 주요 랜더링 과정의 간략한 소개입니다

항목1:  App.js -> DaumMapComponent_createMap실행(생략) 
<br/>
항목2: SearchBar을 통한 폼 입력 후 제출(*지도가 처음 그려집니다)
<br/>
항목3: map객체가 _createMap에서 생성됨에 따라 _coord2Address 등 기타 컴포넌트들의 동작이 유효해집니다(코드 내 조건문 참고)
<br/>
항목4: 생성된 지도에서 drag이벤트 발생(render-trigger : 부모인 _createMap을 통해 받은 handleRemap()실행)
<br/>
항목5: _createMap을 제외한 나머지 컴포넌트들은 _createMap의 state변화에 따라 영향을 받습니다.

## 검색창을 활용해 지도 실행 및 이동시키기

> DaumMapComponent_createMap.js와 SearchBar.js 를 참고하세요

다음 지도 docs : <a href="http://apis.map.daum.net/web/sample/basicMap/">이동하기</a>
<br/>
구현 코드 : 
<br/>
<a href="https://github.com/KisungKim/ReactWithDaumMap/blob/master/DaumMapComponent_createMap.js">createMap으로 이동하기</a>
<br/>
<a href="https://github.com/KisungKim/ReactWithDaumMap/blob/master/SearchBar.js">SearchBar으로 이동하기</a>

## 현재 지도의 중심좌표를 기준으로 행정동 주소와 위도 경도를 받아오기

> DaumMapComponent_createMap.js와  DaumMapComponent_coord2Address.js 를 참고하세요

다음 지도 docs : <a href="http://apis.map.daum.net/web/sample/mapInfo/">이동하기</a>
<br/>
구현 코드 : <a href="https://github.com/KisungKim/ReactWithDaumMap/blob/master/DaumMapComponent_coord2Address.js">이동하기</a>

## 드래그 이벤트 발생 시, 활용방법 예시

> DaumMapComponent_createMap.js와  DaumMapComponent_mapDragend.js 를 참고하세요

다음 지도 docs : <a href="http://apis.map.daum.net/web/sample/addMapDragendEvent/">이동하기</a>
<br/>
구현 코드 : <a href="https://github.com/KisungKim/ReactWithDaumMap/blob/master/DaumMapComponent_mapDragEnd.js">이동하기</a>

## 지도의 크기를 조정하는 방법 예시

> DaumMapComponent_createMap.js와  DaumMapComponent_resize.js 를 참고하세요

다음 지도 docs : <a href="http://apis.map.daum.net/web/sample/mapRelayout/">이동하기</a>
<br/>음
구현 코드 : <a href="https://github.com/KisungKim/ReactWithDaumMap/blob/master/DaumMapComponent_resize.js">이동하기</a>

## 현재 지도를 static이미지로 받아오기

> DaumMapComponent_createMap.js와  DaumMapComponent_staticMap.js 를 참고하세요

다음 지도 docs : <a href="http://apis.map.daum.net/web/sample/staticMapWithMarker/">이동하기</a>
<br/>
구현 코드 : <a href="https://github.com/KisungKim/ReactWithDaumMap/blob/master/DaumMapComponent_staticMap.js">이동하기</a>
