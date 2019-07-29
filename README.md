# AndroidLayouts

> 자바로 연습했던 레이아웃들을 코틀린으로 바꿔서 연습해보자!

<br>

## WebView

💁 06/25 코틀린으로 간단한 웹뷰를 띄워보는 프로젝트 ( 블로그 사이트를 띄웠다. )

<br>

## FirebaseInit

💁 07/09 ~ Firebase 프로젝트와 안드로이드 프로젝트를 연동시켜보는 프로젝트("Firebase로 안드로이드 SNS 앱 만들기(하울)" 책 참고)

Firebase에 대해서 간단하게 알아보려면 [여기](https://choheeis.github.io/firebase/2019/07/09/firebase.html)를 보면 도움이 된다!

<br>

- __1. 시작하기__

    안드로이드 스튜디오를 통해 새 프로젝트를 생성한다.

    (프로젝트 이름을 FirebaseInit 으로 설정하였다.)

    <br>

- __2. 파이어베이스 홈페이지 접속하기__

    파이어베이스를 사용해야 하므로 공식 홈페이지에 들어가본다.

    공식 홈페이지 정중앙에 시작하기 버튼이 있으므로 클릭하였더니 [Firebase콘솔](https://console.firebase.google.com/) 사이트로 접속되었다.

    바로 보이는 프로젝트 추가 버튼을 클릭한 후, 프로젝트 이름란에 FirebaseInit을 적고, 애널리틱스 위치를 대한민국으로 바꾼 후, 프로젝트 만들기를 클릭하였다.

    <br>

- __3. Android 앱에 Firebase 추가하기__

    프로젝트 만들기를 클릭하면 아래와 같은 FirebaseInit 프로젝트 콘솔 사이트로 연결된다.

    ![firebase01](https://user-images.githubusercontent.com/31889335/61611493-e515e500-ac96-11e9-94e8-4f17c0bc54bb.PNG)

    화면 중앙에 __앱에 Firebase를 추가하여 시작하기__ 문구가 보이고, 안드로이드 버튼을 클릭했다.

    그랬더니, 앱 등록을 위해 정보를 입력해야 하는 창이 보여졌고, 앱 닉네임과 디버그 서명 인증서라는 란은 선택사항이므로 Android 패키지 이름만 작성하였다.
    
    Android 패키지 이름은 1번에서 생성한 프로젝트의 패키지 이름을 작성하였다.

    구성 파일을 다운로드하고, Android 프로젝트 폴더에 추가하는 방법이 친절하게 설명되어 있는 창이 떠서 그대로 따라하였다.

    그 다음, Firebase SDK를 안드로이드 스튜디오에 추가하는 방법이 나와서 이것도 그대로 따라하였다.

    여기까지 하면 앱이 Google 서버와 통신했는지 확인하는 작업을 거쳐 (오래걸린다..? 5분 정도 기다리다가 그냥 이 단계 건너뛰기를 눌렀다.) 안드로이드 프로젝트와 연동이 된다!
    
    <br>

- __4. 오류 해결하기__

    3번까지 했더니 오류가 떴다.. 구글링을 엄청 해서 [여기](https://developer.android.com/jetpack/androidx/migrate) 라는 사이트에 나와있는대로 두 가지 속성을 true로 추가해줬더니 해결되었다!
    
    <br>

    > 여기까지 해서 안드로이드 프로젝트과 Firebase를 연동시키는 작업을 끝냈다!
    
    <br>

## FlutterCodelab

💁 07/27 GDG Campus Korea에서 주최한 Try! Flutter세미나에 참가하여 실습해본 Flutter 코드랩 프로젝트

[코드랩1](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt1/index.html?index=..%2F..index#1) 과 [코드랩2](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2/index.html?index=..%2F..index#0) 을 따라해보았다.

