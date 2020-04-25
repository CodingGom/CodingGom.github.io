---
layout: post
title: "[Unity] UIWebView Deprecated 이슈 해결법"
date: 2020-04-25
excerpt: "안드로이드의 4대 구성요소(컴포넌트)에 관하여."
tags: [Unity]
comments: false
sitemap :
changefreq : daily
priority : 1.0
---

![warning](/assets/img/uiwebview/warning.PNG)

어느날 부턴가 iOS AppStore에 앱을 제출하면 위와같은 경고 메일이 오게되었다.
요약하자면 "UIWebView 는 앞으로 사용되지 않으니 해당 이슈 처리를 미리 미리 해달라" 인데...
사실 처음에는 원인 파악이 힘들었다. 일반적으로 Deprecate 되는 API나 라이브러리등을 제거하거나 마이그레이션 할때는 사용되는곳을 찾는게 우선이다.

### 첫번째 해결책
해당 앱을 만들때 사용한 Third party SDK나 패키지들을 살펴 보았다. 이부분에서는 쉽게 찾을 수 있었다. Firebase와 Unity Ads에서 UIWebView 를 사용중이었다. 다행이도 SDK와 패키지 버전을 업하여 쉽게 해결할수... 있을줄 알았다. 분명 버전업을 통해서 해당 모듈들에서는 UIWebView를 사용하지 않고 더이상 XCode와 Unity 에디터에서는 UIWebView의 흔적을 찾아 볼수 없었다.

![firebase](/assets/img/uiwebview/firebase.PNG)
[출처 : Firebase release note]

![unityads](/assets/img/uiwebview/unityads.PNG)
[출처 : Unity Ads release note]

참고로 함께 일하는 개발자분의 말씀으로는 Google Admob, Vungle SDK 에서도 발견됬었다고 한다.

![admobios](/assets/img/uiwebview/admobios.PNG)
[출처 : Google Admob release note]

Admob의 경우 iOS SDK 7.55.0 버전에서 해결했다고 한다. 해당 버전 이상을 빌트인 하고 있는 Unity SDK를 사용하면 된다.

![admob](/assets/img/uiwebview/admob.PNG)
[출처 : Google Admob release note]

Vungle 또한 마찬가지이다. iOS SDK 6.4.3 에서 UIWebView 이슈를 해결했다고 하니, 해당 버전 이상이 빌트인된 플러그인을 사용하도록 하자. 현시점 기준으로 release된 Unity 플러그인은 6.4.3이상이 빌트인 되어 있다.

![vungleios](/assets/img/uiwebview/vungleios.PNG)
[출처 : Vungle git release note]


### Third party가 아니라면...
Unity 내부에 내장된 빌트인 iOS 관련 부분이 아닐까 생각했다. 그래서 찾아 보았다.

![before](/assets/img/uiwebview/before.png)

위 이미지는 유니티로 빌드한 XCode프로젝트의 모든 파일들중 "UIWebView"라는 심볼을 찾은 결과이다. 이미지에서 보듯이 'libiPhone-lib.a' 라이브러리와 'UnityAds' 모듈에서 "UIWebView"를 사용중이었다. 라이브러리 쪽은 빌트인된 녀석이고 모듈은 기본으로 설치된 유니티 패키지였다...

UnityAds는 패키지 자체를 삭제하면 해결될것이다.(물론 본인은 광고모듈을 사용하지 않기에 삭제한것이다.) 그렇다면 libiPhone-lib.a 는? 이녀석은 빌트인된 녀석이라 빌드할때마다 갱신될 것이다. 애초에 원본을 교체하거나 해결된 버전의 Unity 엔진을 사용하는 방법밖에는 없을것이다. 본인은 두가지 방법을 모두 시도해 보았다.

libiPhone-lib.a 의 URLUtility.o 라는 오브젝트 파일에서 "UIWebView"를 사용중이라는 정보를 접한뒤 해당 오브젝트 파일을 새로 빌드하여 교체해 보았지만...

Unity 자체 API 인 Application.OpenURL() 이 정상적으로 작동하지 않았다. 역시 함부로 건드는게 아니다.

결국 선택한건 엔진의 버전업... 아직 개발단계에서 부담스러운 선택이었지만 정책상의 이유로 어쩔수 없이 업데이트를 강행할수 밖에 없었다.

Unity 2019.3.10f1 이후 에서는 UIWebView 이슈가 해결된 상태이다. 혹은 2018LTS 버전에서도 해결되어 있다고 한다.

![after](/assets/img/uiwebview/after.png)
