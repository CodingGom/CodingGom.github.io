---
layout: post
title: "[전산학] 운영체제(OS) - Context switching(컨텍스트 스위칭, 문맥교환)"
date: 2019-07-11
excerpt: "Context switching에 관하여 "
tags: [전산학]
comments: false
sitemap :
changefreq : daily
priority : 1.0
---

간혹 그런일이 있지 않은가? 무언가 일을 하고 있었는데 다른일이 치고 들어온다. 이때 치고들어온 일을 처리하고 이전에 하던일을 다시 하려고 하면, 무슨일을 했었는지 어디까지 했는지 잘 기억이 안나는 경우말이다. 이럴때 내가 무슨일을 했는지, 어디까지 했는지, 다음은 무얼하면 되는지 등을 메모라도 해놨다면 참 좋았을텐데 하고 후회하기도 한다.

 멀티프로세스 환경의 CPU또한 마찬가지이다. CPU가 어떤 프로세스를 실행하고 있는 상태에서 인터럽트가 왔을때, 우선순위가 높은은 프로세스라면 지금 수행하고 있는 작업을 중단하고 요청온 프로세스를 수행해야 한다. 이때 아무런 메모도 없이 수행한다면? 나처럼 아무것도 기억 못하고 후회(?)를 하게 될것이다. 그렇기 때문에 프로세스의 상태를 정의한 구조인 Context를 두고, 서로 교체(switching) 함으로써 프로세스를 이어서 수행할수 있게 되는 것이다.

 ---

 ### Context switching

CPU가 특정 프로세스를 실행하기 위한 프로세스의 정보들이 바로 Context이다. 이러한 Context는 Process Control Block(PCB)라는 곳에 저장된다.

![pcb](/assets/img/contextSwitching/pcb.png)
[출처 : Nesoy Blog, https://nesoy.github.io/articles/2018-11/Context-Switching]

PCB에는 프로세스상태, 다음 실행할 명령어의 주소값, 레지스터 등의 정보가 저장된다.

Context switching 이 일어날때 현재 진행하고 있는 프로세스의 정보들을 이 PCB에 저장을 하고, 다음에 진행할 프로세스의 상태를 읽어 진행하게 되는것이다. 잠시 다른일을 할때 현재 하고 있는 일의 메모 한다고 생각하면 쉽게 이해가 되지 않을까?

 ### Context switching 은 언제 일어나는가?
 Context switching 이 일어날때는 Interrupt 가 발생할때 이다. Interrupt는 추후 자세히 설명하겠지만, 간단히 말하자 외부 프로세스에서 예외 발생에대한 처리가 필요한경우 CPU에게 알려주는 것을 말한다. 물론 모든 Interrupt에 대해 발생하는게 아니다. 입출력 요청, CPU 사용시간 만료, Interrupt 처리 대기, Fork child process(자식 프로세스 생성) 등 여러가지가 있다. 모든 인터럽트가 발생할때  Context switching이 일어난다는건 잘못된 사실이다.

 크게 다음과 같은 3과정을 거친다.

  1) Interrupt 발생 및 처리 루틴
  현재 진행되고 있는 프로세스의 상태를 PCB에 저장

  2) 프로세스 스케쥴러 루틴
  다음에 실행할 프로세스를 선택

  3) Dispatch 루틴
  선택한 프로세스의 상태를 PCB를 통해서 복구

위 3과정(Context switching)을 거치면, 잠시 일을 중단하고 다른 일을 처리할수 있게 되는 것이다. 참고로 위 루틴에서 설명한 행위들에서는 오버헤드가 발생하게 된다. 그말인 즉, Context switching이 일어날때에는 아무런 작업을 할수 없다는 것이다.
