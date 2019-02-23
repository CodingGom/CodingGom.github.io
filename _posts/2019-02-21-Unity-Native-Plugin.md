---
layout: post
title: "[Unity] Unity mobile native plugin"
date: 2019-02-21
excerpt: "Unity 의 native plugin에 관하여"
tags: [Unity]
comments: false
---

 Unity로 모바일 게임혹은 앱을 만들다 보면, 모바일 기기가 제공하는 기능들을 사용해야 하는
경우가 있다. 혹은 C++로 개발된 라이브러리를 이용해야 하는 경우가 있다.
이번에는 mobile native로 예를 들어 보겠다. mobile native 기능의 예를들어 보자면
일정시간이 되면 오는 Push notification, 카메라등이 그러하다.
일부는 Unity에서 API등으로 제공하지만, OS별로 차이나 제공하지 않는 기능들은 어떻게
이용해야 할까? 그냥 안써야 하나? 아니다. 이것을 위해 존재하는게 Plugin 이다.

## Plugin ?
![plugin](/assets/img/mobileNativePlugin/plugin.jpg)
[출처 : 라인게임기술개발블로그, https://m.blog.naver.com/PostView.nhn?blogId=linegamedev&logNo=220104074319&proxyReferer=https%3A%2F%2Fwww.google.com%2F]

위 그림은 Plugin을 간단하게 그림으로 표현한것이다. 위 그림을 요약해 보자면 다음과 같다.
>Native 기능을 Plugin으로 만들어 Brige를 이용하여 Unity에 가져오는것

보통 Android와 iOS 개발을 많이 한다. 각각 Native 앱은 해당 플랫폼에 맞는 언어와 IDE로
개발한다. Android는 Android studio로 Java혹은 Kotlin으로 개발을,
iOS는 Xcode로 Object- C / Object - C++, Swift 를 기본으로 개발을 한다. Plugin은 각각
언어로 제공되는 Native 기능을 이용하기 위한 API나 구현한 코드를 외부에서 호출하도록 라이브러리화한 것이라고 할 수 있다. 이러한 Plugin을 Native plugin이라고 한다. 이말고도 Managed plugin이라고 있는데 이는 나중에 알아보도록 하겠다. 실제로 Unity공식 메뉴얼에서 이러한 Plugin는
"플랫폼별 고유 코드 라이브러리" 라고 정의 하고있다.

---

## Brige
강을 건너 반대편으로 가고 싶다면, 다리(Brige)를 건너야 한다. Plugin 을 쓰기 위해서도 마찬가지로
이다. Native로 작성된 코드를 Unity에서는 바로 호출 할 수 없기때문에 C#으로 작성된 다리(Brige)를
호출해서 Native 코드가 Unity쪽으로 건너올 수 있게 해줘야 한다.

---

## iOS 코드
iOS코드는 .a(Xcode lib), .m(Object-C), .mm(Object-C++), .c(C언어), .cpp(C++언어)
파일로 만들수 있다. 각각 작성한 코드를 Unity 프로젝트의 Assets/Plugins/iOS 폴더에 넣어두면
빌드시 Xcode에 자동으로 포함된다. 아래는 예시다.
> // iOS 코드 파일, 예시 .mm
// Native 코드를 호출할 함수
(void) PluginFunc()
{
  ...
}
> // .cpp 과 .mm 의 Name Mangling 문제때문에 작성.
// .c나 .m 일경우 필요 없음.
// C# Brige 에서 호출할 외부 함수 정의
>extern "C" {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;void _PluginFunc(){ PluginFunc(); }
>}_

 * 위 코드 주석에서 언급한 Name Mangling 다른 포스트에서 다루도록 하겠다.

Native 코드를 작성했으니 Brige가 필요하다. Brige에서는 extern 함수를 호출하는 방식으로
연동해주면 된다.
> // DllImport 를 사용하기 위하여 필요한 namespace
> ...
> using System.Runtime.InteropServices;
>
> [DllImport ("__Internal")]
> public static extern void _PluginFunc();
> ...
> // C# 스크립트에서 함수 호출.
> _PluginFunc(); // iOS 코드에서 정의한 extern 함수

의외로 간단하지 않은가?

---

## Android 코드
Android 코드는 .arr(안드로이스스튜디오 Plugin 아카이브), .jar(Java Plugin 아카이브 )
를 만들어서 Unity 에서는 Java class를 호출하는 방식으로 연동한다. iOS 와 마찬가지로
각각 작성한 코드를 Unity 프로젝트의 Assets/Plugins/Android 폴더에 넣어두면 된다.
차이가 있다면 Android는 apk까지 뽑을수 있기때문에 AndroidMenifest 를 포함해야 한다.
즉 Android 프로젝트를 위한 모든 설정이 포함되어야 한다는 것이다. 사실 iOS 보다 접착성은 \
좋지만 구현이 좀더 귀찮지 않나 싶다.

> // Java class
> public class PluginAndroidClass{
> ...
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; void PluginFunc() { ... }  
>}

위아같이 class를 작성하여 .jar 이나 .arr 파일로 추출한후 Assets/Plugins/Android에
넣어준다. 그리고나서 Brige를 작성하여 해당 class를 호출하면 된다.
> // C# Brige
> // void Func(){
> ...
>  // UnityPlayer와 연동 되어있으므로
>  AndroidJavaClass pluginClass = new AndroidJavaClass("Unity패키지명+클래스명");
>  pluginClass.Call("PluginFunc");
>}

실제로 위 과정외에 .jar, .arr 로 추출하는 거나 AndroidMenifest 설정이 귀찮다.

---

## Native에서 Unity 함수 호출하기
Unity에서는 각각 Native 코드를 호출할수 있는데 반대로는 어떨까? 반대도 가능하다.

iOS 코드에서는
> void UnitySendMessage(const char* obj, const char* method, const char* msg);​

Android 코드에서는
> import com.unity3d.player.UnityPlayer;​
> public static void UnitySendMessage(String obj, String method, String msg);​

SendMessage와 같은 사용법을 가진다.

예전에 Asset들은 모바일기기의 Native 기능을 어떻게 가져다 썼는지 궁금했는데,
이와 같이 연동할수 있다는 사실을 알게된후로 궁금증이 가셨던 기억이 난다.
