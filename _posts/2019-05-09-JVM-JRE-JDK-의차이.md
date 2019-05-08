---
layout: post
title: "[Programming] JVM, JRE, JDK"
date: 2019-05-09
excerpt: "JVM, JRE, JDK 에 관하여"
tags: [Programming]
comments: false
sitemap :
changefreq : daily
priority : 1.0
---

안드로이드를 처음 접하는 사람들, Java를 처음 접하는 사람들은 JVM, JRE, JDK에 관하여 많이들 헷갈려 한다고 한다. 풀네임을 알면 사실 어려울게 없지만, 처음부터 풀네임을 유추하는 사람이 몇명이나 될까? 오늘은 이 세가지가 각각 어떤것인지 포스팅 하려한다.

---

### JVM
Java Virtual Machine의 약자. 해석하면 자바 가상머신 이다. 이녀석은

1) Java소스코드로 부터 만들어지는 바이너리파일의 바이너리 코드를 읽고

2) 바이너리 코드가 정상작동하는지 검증하고

3) 검증된 바이너리 코드를 실행해서 프로그램을 구동한다

위 과정을 통하여, Java 바이너리 코드가 실행될수 있는 런타임 환경을 제공하는 규격이다. 이름에서 볼수 있듯이 실제 기계가 아닌 추상적인 기계이다. Java 프로그램은 JVM 위에서 바이너리 코드로 작동하기 때문에 OS 관계없이 작동한다.

여담으로 JVM 은 OS별로 따로 존재한다. 윈도우용, 리눅스용, Mac용 등등...

---

### JRE
Java Runtime Environment의 약자. 위 설명만 보면 JVM만 있으면 될거 같지만 실상은 그렇지 않다. 라이브러리 Java를 실행하기 위한 여러 파일들도 존재 할것이다. JVM과 이러한 라이브러리나 파일들을 통틀어 JRE라 한다. 즉, Java를 실행시키기 위한 환경인것이고 JVM은 JRE의 일부라는 것이다.

---

### JDK
Java Development Kit의 약자. JRE에 개발에 필요한 도구들을 포함한 것이다. 기본적으로 Java를 실행해야하기때문에 JRE가 기본이 되야하고 개발에 필요한 툴등을 제공해주는 것이라고 생각하면 된다.

![jdk](/assets/img/java/jdk.jpg)
[출처 : 나만의공간, https://m.blog.naver.com/]

요약하면 위 그림과 같다.

---

### NDK 와 JNI
추가로 NDK 와 JNI에 관해서다. Native Developement Kit, Java Native Interface 의 약자이다. 이름에서 보면 알수 있듯이 Native 코드와 관련된 녀석들이다.

 - NDK : C/C++로 안드로이드 앱을 개발할 수 있도록 도와주는 도구.
 - JNI : Java와 NDK로 만들어진 소스들을 서로 호출하고 호출될 수 있도록 인터페이스를 제공해주는 녀석. 중간 다리라고 생각하면 쉽겠다.

이라고 생각하면 된다. Java가 성능이 많이 떨어지던 시절 많은 사람이 C/C++로 안드로이드 앱을 개발하기 원했고 이때 나온게 NDK이다. 지금은 Java와 NDK로 만들었던 녀석들을 연결해서 쓸수까지 있으니... 좋아진거겠지?

여담으로 유니티에서 il2cpp 을 스크립트 백엔드로 이용하려면(64bit 아키텍쳐 빌드등) 유니티 엔진 버전에 맞는 NDK가 필요하다.
