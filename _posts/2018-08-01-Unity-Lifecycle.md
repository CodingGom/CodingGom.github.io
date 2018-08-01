---
layout: post
title: "[Unity] Unity Lifecycle"
date: 2018-08-01
excerpt: "유니티의 라이프 사이클"
tags: [Unity]
comments: false
---

## Unity Lifecycle

 유니티를 다뤄본 사람이라면, 스크립트를 작성할때 Awake()함수나 Update()함수등을 본적이
있을것이다. 이러한 함수들은 호출하지 않아도 자동으로 호출되는 함수들이며, 이들의 호출되는
순서를 생명주기(Lifecycle)이라고 한다.

다음은 유니티 공식문서에 나와있는 이벤트함수 호출 플로우차트이다.
![monobehaviour_flowchart](./assets/img/unitylifecycle/monobehaviour_flowchart.png)

[출처 : 유니티 공식문서_https://docs.unity3d.com/kr/530/Manual/ExecutionOrder.html]

하나하나 살펴보자

* Editor 부분
  1. Reset() : Reset명령이 주어졌을때나 스크립트가 개체에 처음 연결될때 호출

* Initialization 부분
  1. Awake() : 프리팹 인스턴스화 직후에 호, 스크립트가 활성화될때 가장 처음 호출되는 함수이다.
  2. OnEnalbe() : 이 함수는 오브젝트를 활성화 한 직후에 호출됨.
  3. Start() : 활성화 된 후, 첫프레임에서 한번 가장먼저 호출됨.

* Physics
  1. FixedUpdate() : Update()함수가 매 프레임마다 호출된다면, 이 함수는 설정된 일정 주기
    에 따라 호출됨. 주로 프레임에 독립적인 물리연산등의 계산에 쓰인다.
  2. OnTrigger~ : 충돌체가 트리거일(isTrigger == true)경우 발생되는 이벤트 함수. 충돌후
    물리효과가 적용될 필요가 없는경우에 사용한다.
  3. OnCollider~ : 충돌체가 콜라이더일(isTrigger == false) 경우 발생되는 이벤트 함수.
    충돌후 물리효과가 적용되야 할경우 사용한다.

* InputEvent 부분
  마우스, 키보드, 터치 이벤트가 호출되는 부분

* Logic 부분
  1. Update() : 매 프레임마다 호출된다. 주로 프레임단위의 갱신에 사용된다.
  2. LateUpdate() : Update()함수가 호출된 직후 호출된다. Update()에서 처리후 무언가를
   처리할경우 이곳에 처리하면 된다.
  3. yield ~ : 비동기로 처리되는 코루틴들은 Update()가 반환된후 처리된다.
  => 단 yield WaitForFixedUpdate()의 경우는 Physics 부분임

* Decommissioning 부분
  1. OnDestroy() : 오브젝트 생존기간의 마지막부분에 호출. 이 함수의 호출로 생명주기가 끝난다.

굵직한 몇몇 중요 함수들과 부분만 설명해보았다. 순서 자체를 요약해보면

 > 에디터실행 -> 오브젝트 초기화 -> 물리업데이트 -> 입출력 이벤트 -> 로직 갱신
  -> 렌더링 -> 비활성화 -> 파괴

이다. 이처럼 라이프 사이클을 알면, 어떤부분에서 무슨 처리를 해야하며 프로그래밍의 순서를
설계하는데 도움이 될것이다.

여담으로 이 함수들이 스크립트상에서 존재한다면, 정의되지 않았더라도 호출된다. 조금이라도
성능에 보탬이 되고 싶다면, 비어있는 이벤트 함수들은 스크립트 상에서 삭제하도록 하자.
(호출하지 않아도 호출된다는것과는 별개의 이야기이다.)
