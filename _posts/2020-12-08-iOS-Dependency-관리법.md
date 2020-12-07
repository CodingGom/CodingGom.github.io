---
layout: post
title: "[Programming] iOS Dependency (의존성), cocoapods (코코아팟)"
date: 2020-12-08
excerpt: "iOS 프로젝트의 라이브러리 Dependency (의존성) 관리 매니저, Cocoapods 에 관하여"
tags: [Programming, iOS]
comments: false
sitemap :
changefreq : daily
priority : 1.0
---

### 들어가기에 앞서

해당 포스팅은 cocoapods 의 사용법에 관한 포스팅이 아닌, 개념에 관한 내용이다. 사용법은 추후 포스팅할 예정이다.

---


### cocoapods

iOS 개발을 하는 사람들, 조금이라도 iOS 개발에 관련되는 사람들 이라면 필수적인 녀석이 있다. 바로 cocoapods 이라는 의존성 관리 매니저 이다. cocoapods 공식 홈페이지에 소개된 내용이다. (2020년 12월 기)


> CocoaPods is a dependency manager for Swift and Objective-C Cocoa projects. It has over 79 thousand libraries and is used in over 3 million apps. CocoaPods can help you scale your projects elegantly.
>

내용인 즉, "어마어마하게 많은 라이브러리 사이의 종속성(의존성)을 관리해주는 아주 멋진 녀석이다." 라는 것이다.

---


### cocoapods 은 어떻게 의존성 관리를 하는가?

<p><a href="https://codinggom.github.io/Dependency/">이전 포스팅 </a> 에서 의존성을 관리하긴 하는데, 프로젝트는 어떻게 알고 관리를 하는지 의문을 표한적이 있다. cocoapods 은 Podfile 이란 것을 이용한다. 구조는 간단하다.

| 1) Podfile 은 최종 프로젝트를 생성해주는데 필요한 정보가 기재된 파일이다.

| 2) Podfile 에는 라이브러리 이름, 버전, 소스, 종속시킬 타겟 등등... 의 정보가 기재되어 있다.

| 3) 종석성이 관리되기전 Xcode 프로젝트에 Podfile 을 생성해준다.

| 4) 터미널 명령어 or 스크립트 등을 이용하여 해당 Podfile 기반의 Workspace (Xcode 프로젝트 형식)를 생성해준다.

| 5) 생성된 Workspace 가 최종 프로젝트 파일이다. 이 파일로 앱을 빌드해 배포하면 된다.


위 단계중, 개발자가 신경써야 할것은 Podfile 이다. Podfile 이외는 고정된 과정이며, Podfile 이 어떻게 구성되어 있는지에 따라 모든게 결정된다.


---


### Podfile 예시

아래는 간단한 Podfile 예시 이다.

```
platform :ios, '10.0'

source 'https://github.com/CocoaPods/Specs.git'


target 'Unity-iPhone' do
    pod ‘CustomLib’, '1.0.0'
end

```

간단하게 해석을 해보면, 프로젝트 내에 'Unity-iPhone' 타겟에 'CustomLib' 의 '1.0.0' 버전을 의존성으로 걸어준다. 해당 라이브러리는 'https://github.com/CocoaPods/Specs.git' 에서 가져온다.

이처럼 프로젝트에 사용할 구성을 Podfile 로 만들어주는 과정을 해주면 된다.


---


### 마무리

사실 cocoapods 을 사용하지 않아도 프로젝트를 생성해 임의로 관리하며 앱을 출시까지 할수 있다. 본인도 처음 프로젝트를 시작할때는 사용하는 라이브러리도 적고, 관리할 버전도 많지 않아(배우는 단계라) 임의로 한적이 있다. 하지만, 점점 규모가 커질수록 관리가 버거워지기 시작하고 휴먼미스도 많이 생기게 되었다. 어쩌면 이러한 관리 방식은 선택이 아닌 필수가 아닐까 싶다. 나중에는 cocoapods 을 설치하고, 배포하는 방법도 포스팅 해볼까 한다.
