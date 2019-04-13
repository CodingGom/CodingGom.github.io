---
layout: post
title: "[Programming] iOS 앱간 데이터 공유 Keychain sharing (키체인 쉐어링)"
date: 2019-04-14
excerpt: "메모리 단편화에 관하여"
tags: [Programming]
comments: false
sitemap :
changefreq : daily
priority : 1.0
---

이전에 안드로이드 앱간 데이터 공유방법중 CP(ContentProvider) 와 CR(ContentResolver)를 이용하는 방법에 대하여 포스팅한적이 있다. 이번에는 iOS 에서는 어떻게 하는지 알아보려고 한다. 이 포스트 역시 코딩보다는 간단한 개념을 포스팅하는게 주 목적이다.


### Keychain
Keychain(키체인) 이라는 것은 iOS장치상에서 암호화 정보를 저장하기 위해 제공되는 Framework 이다. 일반적인 로컬 데이터는 앱이 삭제되면 같이 삭제되지만, 이 Keychain에 저장된 정보는 앱의 삭제와 상관없이 저장이 된다. 여러 정보들이 저장되는 Keychain은 하나의 암호화된 컨테이너이다. 이러한 컨테이너에 저장된 정보를 암호화 하고 복호화 해서 안전하게 사용할수 있도록  Keychain services API를 제공한다. 해당 API를 사용하는방법에는 다양한게 있지만 가장 간단한거라고 생각되는것은 KeychainItemWrapper class를 이용하는게 아닐까 한다.

![keychainitem](/assets/img/datashare/keychainitem.png)
[출처 : 이동건의 이유있는 블로그, https://baked-corn.tistory.com/69]

위 그림은 Keychain item 을  Keychain services API를 이용하는 방법을 도식화 한것이다. Keychain item이란 Keychain 이라는 컨테이너에 저장되는 단위이다. 즉, 하나의 컨테이너에 들어가는 요소라고 생각할수 있다. 결국, Keychain 컨테이너에 저장된 item을 사용한다... vector나 list 사용과 같지 않나?

KeychainItemWrapper class의 사용법의 예시는 아래와 같다.
![keychainwarpper](/assets/img/datashare/keychainwarpper.png)
[출처 : 10Apps, https://10apps.tistory.com/139]


### Keychain Group
iOS 앱을 만드는 사람들이라면 Apple 개발자 등록을 했을것이다. 개발자등록을 하고나면 Team ID(App ID Prefix라고도 부름)라는걸 봤을것이다. Keychain Group은 이 Team ID가 같은 앱들끼리 Keychain을 공유하게 해주는 것이다. 이 Keychain Group은 Xcode에서 빌드 설정을 통해 설정할 수 있다. 이때문에 Keychain Sharing은 결국 같은 개발자가 개발한 앱(Team ID가 같아야 하므로)들 사이에서만 데이터가 가능하다는 것이다. 물론 외부 사람이 같은 인증서를 쓴다면... 그러진 말자.
