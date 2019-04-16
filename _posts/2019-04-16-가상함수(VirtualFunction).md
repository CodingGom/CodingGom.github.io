---
layout: post
title: "[Programming] 가상함수(Virtual function or Virtual method)"
date: 2019-04-16
excerpt: "가상함수(Virtual function)에 관하여"
tags: [Programming]
comments: false
sitemap :
changefreq : daily
priority : 1.0
---

객체지향 프로그래밍에서 빼놓을수 없는 내용중 하나가 가상함수가 아닐까 한다. 왜냐하면 가상함수는 객체지향언어의 '다형성'을 가능하게 해주기 때문이다.

### 가상함수
가상함수란 상속하는 클래스 내에서 같은 형태의 함수를 오버라이딩(overriding) 할수 있도록, 임시로 선언해놓은 함수를 말한다. 즉, '일단 이런 함수가 있으니 상속하는 클래스에서 재정의 해서 써라.' 라는 것이다. 예를 들어보자 사자, 원숭이, 알파카 등 모든 동물은 자신들의 주식을 먹는다. 단지 먹는것이 육식인지 초식인지 잡식인지의 차이일 뿐이다. 행위 자체는 같은데 재료가 다르다고 모든 행위를 다르게 정의 해야하는가? 솔직히 좀 낭비 아닌가 싶다. 사자, 원숭이, 알파카등을 '동물'이라는 상위 클래스를 상속받는 자식 클래스로 만들어서 행위는 부모에 virtual로 선언하고 각각 자식에서 재정의 하면 되지 않을까? 아래의 코드 예시를 보자

![class](/assets/img/virtual/class.png)
> class Animal {
>    void /*non-virtual*/ move(void) {
>        std::cout << "This animal moves in some way" << std::endl;
>    }
>    virtual void eat(void) {}
>};
>
>// The class "Animal" may possess a definition for eat() if desired.
>class Llama : public Animal {
>    // The non virtual function move() is inherited but not overridden
>    void eat(void) {
>        std::cout << "Llamas eat grass!" << std::endl;
>    }
>};

[출처 : 위키백과, https://ko.wikipedia.org/wiki/%EA%B0%80%EC%83%81_%ED%95%A8%EC%88%98]


먹는다는 행위인 void eat() 함수를 Animal class 에서 virtual로 선언하고 자식인 Llama class 에서 오버라이딩 해주었다. 이런식으로 객체지향의 다형성을 가능하게 해준다.


### 가상함수의 작동 원리
그러면 가상함수는 어떻게 저런 행동이 가능한것일까? 키포인트는 '가상함수 포인터' 와 '가상함수 테이블' 이다. 가상함수 포인터(__vfptr)는 가상함수가 선언된 class에 자동으로 생기는 포인터 변수이다. 여담으로 가상함수가 선언된 클래스는 4바이트의 크기가 추가 된다.(포인터 변수의 크기) 이 가상함수 포인터는 가상함수 테이블(vftbl)의 시작주소를 가르킨다. 가상함수 테이블은 실제로 실행될 재정의된 함수들이 맵핑된 테이블이다. 이 테이블 역시 가상함수가 선언되고 오버라이드된 함수가 있다면 컴파일러가 생성한다.

그렇다. 가상함수 포인터를 타고 가상함수 테이블로 가서 맵핑된 적절한 함수를 바인딩해서 호출하는 것이다. 가상함수가 재정의된 클래스의 인스턴스가 생성될때, 가상함수 포인터는 가상함수 테이블을 가르키게 된다. 이 객체가 해당 멤버함수를 호출할때 가상함수 테이블에서 찾아서 호출하면, 재정의된 멤버함수가 호출되는 것이다.

만약 자식클래스에서 가상함수를 재정의 하지 않으면, 부모 클래스에 선언된 virtual 함수가 호출 된다.
여담으로, 가상함수가 몇개가 선언되어있던지 4바이트만 추가 된다. 가상함수테이블만 가르키면 되니까!
