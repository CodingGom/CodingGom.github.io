---
layout: post
title: "[Programming] Name Mangling"
date: 2019-02-24
excerpt: "Name Mangling에 관하여"
tags: [C, C++]
comments: false
---

이전 포스트(2019-02-21-Unity native plugin)에서 Name Mangling을 언급한적이 있다.
이번 포스트는 이 Name Mangling에 관하여 적어보려 한다.

## Name Mangling
이 Name Mangling은 컴파일러가 자기 마음대로 변수나 함수의 이름을 변경하는 것을 의미한다.
함수 오버로딩을 알고 있는가? 함수이름이 같고 리턴값 타입이나 매계인자가 다른경우에도
하나의 이름으로 사용할수 있게해주는 C++의 기능이다.
여담으로 Name Decoration이라고 부르기도 한다.

예를 들어 매계인자로 받은 값을 + 해주는 함수를 작성한다고 하자
>int Add(int a, int b) { return a + b; }

근데, 정수는 위처럼 작성한다 하고... 실수끼리나 정수 + 실수 는?
각각 함수이름을 다르게해서 작성하면? 물론 된다. 하지만 가독성과 유지보수랑은 작별하자.
그냥 Add라는 함수로 리턴값과 매계인자 타입만 바꿔주면 간단할거다.
>float Add(float a, float) { return a + b; }
>
>float Add(int a, float b) { return a + b; }
>
>...

일단 코드는 작성해서 프로그래머는 안다. 저것이 오버로딩이라는것을. 하지만 컴파일러는?
이때 컴파일러는 컴파일시 Add함수를 서로 다른 이름으로 번역한다. 즉, Name Mangling을 해준다.
>int Add(int a, int b) { return a + b; } => Addii
>
>float Add(float a, float) { return a + b; } => Addff
>
>float Add(int a, float b) { return a + b; } => Addif
>...

등등... Namming 방식은 컴파일러마다 다르다고 한다. 어쨌든, 이렇게 임의로 이름을 붙여줘
오버로딩을 가능하게 해준다. 그렇다면 이전에 왜 이것이 문제가 된다고 했을까?

## Name Mangling 피하기
외부 모듈이 C기반일 경우 C++로 작성된 모듈에 접근이 가능할까? 아니 불가능 할것이다.
C는 C++의 Name Mangling 규칙을 모를테니까 말이다.컴파일러가 다를 경우 C++ 모듈
사이에서도 접근이 안된다. 그럼 방법이 없는가? 아니 방법은 있다. Name Mangling을 피하자

Plugin을 만들때 extern함수로 선언해야 했다. extern은 기본적으로 extern "C++"이라는
뜻이다. 즉 C++로 작성되었으며 Mangling 을 진행한다. 외부에서는 코드로 작성된 함수
이름과 바이너리(lib파일)로 뽑혀 읽는 함수의 이름을 다르게 읽을것이다. 결국 어떤건지
모른다는 것이다.

즉, extern "C"로 작성하여 Name Mangling을 진행하지 않고 함수이름을 그대로 읽어 해주는게
공용라이브러리등을 만들때 필수적이라 할수 있다. 이를 안해주면 만든사람에게 욕을...
