# 💻 Retrofit 사 용 하 기 !

> SOPT 25기 안드로이드 - 서버 합동 세미나를 위해 __Retrofit__ 이라는 라이브러리를 배웠다.
>
> 아직 안드로이드의 HTTP 통신에 대해서 공부하지 못해서 통신에 대한 지식 없이 Retrofit 라이브러리가 무엇인지만 공부하며 작성하였다.
>
> SOPT 25기 3차 세미나에서 배운 내용 위주로 작성한 포스팅!
> --> [Retrofit 공식 문서](http://devflow.github.io/retrofit-kr/)
>
> --> [github Rest API 문서](https://developer.github.com/v3/)

<br>

# 💻 Retrofit 이 뭐지?

Retrofit은 안드로이드 http RESTful API를 자바(or 코틀린) 인터페이스 형태로 만들어 쉽게 사용 수 있게 해주는 라이브러리이다!

> 안드로이드 통신에 대해서 좀 더 자세히 공부한 후, Retrofit이 무엇인지 좀 더 자세히 적어보자!

<br>

# 💻 Retrofit 사용법!

0. __안드로이드 프로젝트에 Internet 퍼미션 추가하기__

1. __Retrofit 라이브러리를 프로젝트에 추가하기__

2. __서버 개발자와 필요한 데이터에 대해서 논의하는 시간 갖기__

3. __서버 개발자가 만들어준 API 문서 확인하기__

4. __API 문서를 보고 Retrofit Interface 설계하기__

5. __서버 응답 받아오기__

6. __Retrofit Interface의 실제 구현체 만들기__

7. __Callback 등록하여 통신 요청하기__

<br>

- ## 0. 안드로이드 프로젝트에 Internet 퍼미션 추가하기

    서버와의 통신은 인터넷을 통해 이루어지기 때문에 안드로이드 프로젝트의 Manifest 파일에 Internet 퍼미션을 추가해줘야 한다.

    ![16](https://user-images.githubusercontent.com/31889335/69937744-e695ca80-151e-11ea-9ab3-907837b65b41.PNG)

    <br>

- ## 1. Retrofit 라이브러리를 프로젝트에 추가하기

    [Retrofit 공식 문서](http://devflow.github.io/retrofit-kr/) 를 보면 라이브러리 추가에 관한 정보를 찾을 수 있다!

    ![01](https://user-images.githubusercontent.com/31889335/69706880-f9358b80-113b-11ea-95a8-74c08a03ee1b.PNG)

    위 그림에서 (insert latest version)를 작성하기위해 최신 버전이 몇인지를 찾아봐야 하는데 주로 이런 라이브러리의 최신 버전은 github에 들어가면 리드미에 명시되어 있다.

    > --> [Retrofit github](https://github.com/square/retrofit)

    ![02](https://user-images.githubusercontent.com/31889335/69708064-47e42500-113e-11ea-999e-7dc5d0043db1.PNG)

    <br>

- ## 2. 서버 개발자와 필요한 데이터에 대해서 논의하는 시간 갖기

    <img src = "https://user-images.githubusercontent.com/31889335/69725359-5263e600-1161-11ea-942a-bb4394f573c4.jpg" width = "300"/>
    

    만약, 위와 같은 UI를 가진 엑티비티가 있다면 여기서 필요한 정보는 무엇일까?

    위 화면은 choheeis 라는 github 계정을 가진 유저의 repository 목록과 이에 대한 간단한 정보들을 나타내는 화면이다.

    위 화면에서 repository list에 필요한 정보들을 생각해보면

    __repository 제목__

    __repository 설명__

    __주요 언어__

    들이 필요하다.

    따라서, 이런 데이터들을 서버에 요청하여 받아와야 하는 것이다!

    <br>

- ## 3. 서버 개발자가 만들어 준 API 문서 확인하기

    협업을 하면 서버 개발자가 Rest API 문서를 만들어서 줄 것이다. 그것을 확인하면 되는데 지금은 일단 github에서 제공하는 Rest API 문서를 사용해보자!

    > --> [github Rest API 문서](https://developer.github.com/v3/) 참고

    일단 API 문서를 사용하기 전에 알아둬야 할 것이 있는데 RESTful 통신이라는 것에 대해 간략하게나마 알아야 한다. 
    
    RESTful은 __Representational State Transfer__ 의 약자로, 서버에 존재하는 데이터에 접근하기 위한 규칙들의 모음이다!
    
    URI로 서버에 있는 데이터나 서버의 행위를 식별하고, __GET__, __POST__, __PUT__, __DELETE__ 이렇게 4가지 행위를 할 수 있다. 
    
    > __HTTP Method__ 
    >
    > GET : 단순히 데이터 자체를 요구하는 행위
    >
    > POST : 데이터를 서버에 제출하는 행위
    >
    > PUT : 데이터를 변경하는 행위
    >
    > DELETE : 데이터를 삭제하는 행위

    <br>

    Github Rest API 문서 사이트에 접속해보면 Github REST API의 최신 버전에 대해 다음과 같이 설명해 놓은 것을 볼 수 있다.

    ![03](https://user-images.githubusercontent.com/31889335/69738552-a7abf180-1179-11ea-8b10-868bdae03fd9.PNG)

    위 설명을 읽어보면 현재까지는 v3가 Github REST API의 최신버전이고, https://api.github.com 이라는 서버로 들어온 모든 요청은 이 v3 API가 응답한다고 되어있다.

    이 말은 즉슨! __BASE URL__ 이 https://api.github.com 이라는 것이다. BASE URL은 우리가 데이터를 요청할 서버의 이름을 말한다. 이 BASE URL 뒤에 붙는 url에 따라 서버에 어떤 서비스를 요청하는지가 정해지게 된다.
    
    <br>

    BASE URL을 알아본 후 내가 필요한 API를 추가로 찾아보았더니 다음과 같은 양식을 확인할 수 있었다. 

    ![04](https://user-images.githubusercontent.com/31889335/69778641-74547b80-11e8-11ea-9a67-e5c3f81c404c.PNG)
    
    > --> [User Repository list 제공 API](https://developer.github.com/v3/repos/) 
    
    <br>
    
    위 그림에서 __GET__ 이라는 것이 붙어있으므로 이 URL을 사용하면 서버에 데이터를 요청한다는 것이다. 

    그리고 __/users/:username/repos__ 라는 부분은 BASE URL 뒤에 붙는 url이다. 서버에 어떤 서비스를 요청할 것인지에 따라 이 url이 달라진다. 
    
    이 url은 username에 따라 그 유저의 repository 리스트를 달라는 서비스를 요청하는 url이다.
    
    또, __Parameters__ 라는 제목의 표를 보면 Name과 Type이 기술되어 있는 것을 볼 수 있다. 이 표에 있는 Name과 Type은 __BODY__ 라고 부르며 서버에 어떤 정보를 보낼 것인지에 대한 설명이다.

    <br>

- ## 4. API 문서를 보고 Retrofit Interface 설계하기

    이제 서버에서 작성해준 API 문서를 확인했으니 이 문서를 보고 retrofit을 이용해 서버와 통신을 해야할 차례이다!

    그렇다면 Retrofit이라는 라이브러리를 어떻게 사용해야 하는지을 알아봐야 한다. 

    [Retrofit 공식 문서](http://devflow.github.io/retrofit-kr/) 를 보자!
    
    ![11](https://user-images.githubusercontent.com/31889335/69930538-2c469900-1507-11ea-9087-8d010130ebe3.PNG)

    위 설명을 읽어보면 Retrofit 라이브러리를 사용하기 위해 3가지를 숙지하고 있어야 한다는 것을 알 수 있다.

        1. Retrofit은 HTTP API를 자바 인터페이스 형태로 사용할 수 있다.

        2. Retrofit 클래스로 GitHubService 인터페이스를 구현하여 생성해야 한다.

        3. 각각의 Call은 GitHubService 를 통해서 동기, 비동기적으로 HTTP 요청을 서버에 보낼 수 있다.

    <br>

    일단, 1번의 __"Retrofit은 HTTP API를 자바 인터페이스 형태로 사용할 수 있다"__ 라는 설명은 인터페이스를 하나 만들고, 그 안에 Retrofit 방식을 따른 요청 메소드들을 작성하라는 의미이다. 

    즉, 클라이언트에서 서버에 요청하는 서비스는 여러 개가 있을 텐데 이에 대한 각각의 Retrofit 요청 메소드들을 하나의 코틀린 interface 안에 한꺼번에 작성하여 사용하라는 것이다.

    위 설명에서 볼 수 있는 ![12](https://user-images.githubusercontent.com/31889335/69930682-af67ef00-1507-11ea-9afc-306b9156a604.PNG) 부분이 interface 안에 들어있는 Retrofit 요청 메소드이다!
    
    따라서 코틀린으로도 인터페이스를 하나 만들고, 그 안에 Retrofit 방식을 따른 요청 메소드들을 작성해주면 된다.

    ![07](https://user-images.githubusercontent.com/31889335/69929970-75e1b480-1504-11ea-9588-93ec6a0c9658.PNG)

    안드로이드 프로젝트에서는 코틀린 interface를 위 그림처럼 새로운 클래스를 만드는 방법과 같은 방법으로 만들 수 있다.

    > Kind에서 Interface를 선택해주면 된다!

    <br>

    ![06](https://user-images.githubusercontent.com/31889335/69926439-d8808380-14f7-11ea-8532-a7fa8f1374a9.PNG)

    위 설명은 요청메소드에 대한 설명이다. 어노테이션이라는 것은 자바에서 __@Override__ 처럼 골뱅이 표시와 함께 작성되는 것을 말한다. 주로 함수 윗줄에 작성된다. 위 Retrofit 요청 메소드 예시에서는 __@GET__ 이라는 어노테이션을 사용하였고, 상대 URL인 __/users/list__ 를 괄호 안에서 큰 따옴표로 묶어 나타낸 것을 볼 수 있다. 

    여기서 __상대 URL__ 이라는 것은 BASE URL 뒤에 붙는 URL을 말한다! 

    이제 우리 프로젝트에서 필요한 API문서를 보고 Retrofit 요청 메소드를 작성해보자!

    ![08](https://user-images.githubusercontent.com/31889335/69930115-120bbb80-1505-11ea-8231-384cc8ed60d5.PNG)

    위 그림을 보면서 다시 한번 username에 따라 해당 Repository 리스트를 응답해주는 API를 봐보자.

    GET 이라는 HTTP 메소드를 사용하며 상대 URL은 /users/:username/repos 이다. 여기서 :username은 변할 수 있는 부분이다. 즉, 사용자의 계정에 따라 응답받는 repository 리스트가 달라야 하기 때문에 요청 메소드로 서버에 데이터를 요청하는 상대 URL의 username 자리는 아무 유저 계정이 들어올 수 있도록 변할 수 있어야 한다.

    ![10](https://user-images.githubusercontent.com/31889335/69930322-4338bb80-1506-11ea-90ca-67889ec6b251.PNG)

    위 설명을 읽어보면 Retrofit의 상대 URL에서 변할 수 있는 부분은 { }를 사용하여 감싸야하고, 요청 메소드의 매개변수로 해당 부분에 들어가는 값의 변경이 가능하다고 명시되어 있다. 
    
    따라서, ![09](https://user-images.githubusercontent.com/31889335/69930427-c1955d80-1506-11ea-8b2e-740bb79d58cc.PNG) 이와 같이 요청 메소드의 어노테이션과 상대 URL, 요청 메소드의 매개변수를 작성하면 된다.

    그 다음으로 함수의 리턴 타입을 작성해야하는데 이 때 리턴하는 것은 비동기 통신을 지원하는 Retrofit 객체이다.

    ![13](https://user-images.githubusercontent.com/31889335/69936553-746fb680-151b-11ea-9c21-f969ddb8d44a.PNG)

    따라서 username에 따라 해당 유저의 Repository List를 응답해주는 요청에 대한 요청메소드는

    ![14](https://user-images.githubusercontent.com/31889335/69936652-b993e880-151b-11ea-9115-cba9e86fe5d0.PNG)

    이와 같이 작성되게 된다!

    이렇게 interface 안에 GET, POST, DELETE 등의 요청 메소드들을 작성하는 것을 __Retrofit Interface를 설계한다__ 라고 부른다.

    <br>

- ## 5. 서버 응답 받아오기

    서버에서는 요청에 대한 응답으로 데이터를 json 형태로 보내준다. 하지만 이 json 객체에는 우리가 필요하지 않은 데이터들도 있을 경우가 크다! 

    예를 들어, 우리는 레포지토리 이름, 사용언어만 필요한데 서버에서 받아온 데이터에는 레포지토리 생성 날짜, 레포지토리 설명 등등이 함께 응답될 수 있다는 것이다.

    ![15](https://user-images.githubusercontent.com/31889335/69937361-ed700d80-151d-11ea-8883-c8c2c92ed748.PNG)

    위 json은 list user repository API를 보고 요청을 보낸 것에 대한 json 응답이다. 즉, 필요한 데이터보다 엄청나게 많은 데이터가 응답된다는 것이다!

    따라서, json 에 있는 모든 데이터를 사용할 필요가 없을 경우를 위해 __Gson__ 이라는 라이브러리 프로젝트에 추가하여 Retrofit과 함께 사용하면 된다.

    > --> [Gson 공식 문서](https://github.com/google/gson)

    <br>

    - __Gson이란?__

        Gson 공식 문서를 열어보면 Gson은 자바 객체를 JSON으로 변환해주는 라이브러리라고 소개되어있다. 

        원래 기존 방식은 서버에서 응답받은 json 객체를 프로그래머가 직접 파싱해서 필요한 데이터만 사용했었다.

        하지만 이런 수고를 덜기 위해 Gson 이라는 라이브러리를 사용하면 훨~씬 편한 개발을 할 수 있다!

        Gson이 프로그래머가 파싱해야 했던 부분을 알아서 파싱해주고, 프로그래머가 명시한 필요한 데이터만 응답해주기 때문이다!

        일단 Gson 라이브러리를 사용하기 위해 안드로이드 프로젝트의 build.gradle 파일에 Gson 라이브러리를 추가시킨다.

        > 라이브러리를 추가하는 방법은 Gson 공식 문서를 보면 나와있다.

        Gson을 사용해서 필요한 Json 데이터만 응답받는 방법은 데이터 클래스에 __@SerializedName("json key이름")을 추가해주면 된다.
    
        ![18](https://user-images.githubusercontent.com/31889335/69939464-44c4ac80-1523-11ea-812b-f4ef75087568.PNG)
        ![19](https://user-images.githubusercontent.com/31889335/69939461-442c1600-1523-11ea-9a7f-4e524ebc5d71.PNG)
        ![20](https://user-images.githubusercontent.com/31889335/69939462-44c4ac80-1523-11ea-8937-ada071443a85.PNG)

        위 그림은 깃허브 API 공식 문서에 있는  List user repository에 대한 샘플 json 응답이다.

        위처럼 수많은 json 데이터 중 우리는 레포지토리 이름, 사용 언어, 레포지토리 설명만을 알고 싶다면 해당 json의 키 값을 알고 있으면 된다.

        그 다음, 다음과 같이 해당 데이터들을 정의해준 데이터 클래스에 @SerializedName("키값") 이라는 어노테이션을 대응되는 데이터 선언부 위에 작성해주면 된다.

        ![21](https://user-images.githubusercontent.com/31889335/69939581-8d7c6580-1523-11ea-9d92-ef98864c1a37.PNG)

        이 작업을 마치면 Gson이 위 데이터 클래스에서 SerializedName으로 명시한 name, description, language 데이터들만 응답해줄 것이다!

        그런데 지금까지 한 작업은 Gson 만을 위한 작업이다. 우리는 Gson을 Retrofit 라이브러리와 함께 사용해야 한다. 즉, Retrofit을 사용해서 응답받은 json을 바로 Gson이 변환시켜줘야 한다.

        따라서 Retrofit을 사용할 때, Gson을 사용해서 json을 변환할 것이라는 것을 세팅해줘야 한다.

        이 작업은 다음 단계인 6단계에서 나올 것이다!

    <br>

- ## 6. Retrofit Interface의 실제 구현체 만들기

    4단계에서 만든 Retrofit Interface는 Interface에 불과하다. 즉, 이 Interface에 대한 실제 구현체를 만들어야 Interface에 작성해놓은 요청 메소드들을 진짜로 사용할 수 있게 되는 것이다!

    Retrofit 공식 문서를 읽어보면 

    ![22](https://user-images.githubusercontent.com/31889335/69970128-05b54c00-1561-11ea-838b-ba276e2c6948.PNG)

    위와 같은 설명이 쓰여져 있다. 여기서 GitHubService 라는 것은 Interface로 구현한 클래스의 이름이다. 

    > Retrofit 공식 문서에서 예시로 사용하는 것이 GitHubService 이므로 꼭 인터페이스 이름이나 서비스가 GitHubService일 필요는 없다.

    즉, 인터페이스로 구현한 요청메소드들을 실제로 사용하기 위해서 해당 Interface의 실제 구현체를 만들어야 하는데 이 때 Retrofit 클래스를 이용하여 구현체를 만들 수 있다는 것이다.

    따라서 위 코드는 자바 코드이므로 (Retrofit 공식문서 코드 예시가 자바이다.) 코틀린 스타일로 코드를 바꾸면

    ![24](https://user-images.githubusercontent.com/31889335/70023889-21aa0380-15dc-11ea-9741-c779d5a738bd.PNG)

    이와 같이 된다. 

    여기까지 했다면 이제 5단계에서 언급했었던 Gson을 사용하겠다는 것을 Retrofit에 세팅해주어야 한다.

    Retrofit 공식 문서에는 다음과 같은 설명이 추가로 적혀있다.

    ![25](https://user-images.githubusercontent.com/31889335/70024380-3935bc00-15dd-11ea-9e48-8dc75f3f4318.PNG)

    Retrofit을 만든 회사에서 다양한 컨버터들을 만들었는데 그 중 우리가 사용할 Gson도 있고, Gson을 Retrofit과 함께 사용하기 위해서 추가해야할 라이브러리도 명시되어 있다.

    > Gson이 Retrofit 라이브러리를 만든 회사에서 만든 것이였군..오호

    또, Gson을 사용하겠다는 것을 세팅해주는 방법도 나와있는데 Retrofit 클래스의 멤버함수인 __addConverterFactory()__ 를 사용하면 됨을 알 수 있다.

    ![26](https://user-images.githubusercontent.com/31889335/70024618-0a6c1580-15de-11ea-951c-3c43622b199b.PNG)

    따라서 이렇게 실제 구현체를 수정하였다!

    이렇게 구현체를 만들었는데 이 구현체를 어느 클래스의 어디에 위치시켜야 할까??

    우리가 이 구현체를 사용하여 통신을 해야할 곳은 어플 내에서 아주 많을 것이다! 즉, 지금은 Interface안에 github의 repository list를 응답받는 요청 메소드만 작성해 놓았지만 서버와의 통신이 필요한 수많은 요청 메소드(서비스)들이 하나의 Interface 안에 들어가 있다면 이 Interface에 대한 실제 구현체를 통신이 필요한 곳에서 계속 만들어야한다.

    구현체를 계속 만드는 것은 중복되는 작업이기 때문에 프로그래머들에게는 질색이다!

    따라서 서비스 실제 구현체는 하나만 존재하되 코드 아무곳에서나 접근하고 싶다면 __싱글톤 패턴__ 을 이용하면 된다.

    > 음 싱글톤 패턴이라... 처음 들어본다!!! 다음에 꼭 공부해보자!

    코틀린에서 싱글톤 패턴을 구현하려면 Object를 사용하면 된다. 여기서 말하는 Object는 

    ![23](https://user-images.githubusercontent.com/31889335/70025027-2b813600-15df-11ea-9792-6ff09d02f3da.PNG)

    이렇게 코틀린 클래스를 만들 때 Kind로 지정할 수 있다. 

    ![27](https://user-images.githubusercontent.com/31889335/70025084-523f6c80-15df-11ea-9e50-654e507ec1fb.PNG)

    따라서 이렇게 실제 구현체를 Object(싱글톤 객체)를 사용해서 만들어 놓으면 통신이 필요한 어느 곳에서든지 구현체를 반복적으로 만들 필요 없이 바로 사용할 수 있다.

    <br>

- ## 마지막!. Callback 등록하여 통신 요청하기

    여기서 Callback으로 등록하라는 것이 무슨말인지 먼저 이해해야 한다!

    ![28](https://user-images.githubusercontent.com/31889335/70025863-95024400-15e1-11ea-98d3-0cf1b90fec19.PNG)

    위 설명은 Call과 Callback에 대한 차이를 설명한 것이다. 비동기적으로 Type 객체를 받아온다는 것을 이해하기 위해서는 __동기__ 와 __비동기__ 가 무엇인지 알면 쉽게 이해될 것이다.
    
    일단 동기라는 것은 우리가 어떤 임무를 해야할 때 그 임무를 완수하는 동안 임무 외의 다른 작업들은 모두 stop! 시켜놓는 것이다. 임무가 완수된 순간 멈춰있는 다른 작업들이 다시 실행되는 방식이 동기방식이다.
    
    그렇다면 비동기 방식이라는 것은 동기와 반대 의미임을 알 수 있을 것이다!
    
    비동기 방식은 어떤 임무를 실행할 때 임무 외의 다른 작업들과 동시에 실행시키는 것이다. 즉, 병렬적으로 일을 처리하는 것을 말한다.

    그렇다면 __Callback__ 이라는 것을 이해해보자.
    
    비동기 방식으로 다른 작업들과 함께 실행되고 있는 임무가 어는 순간 완수된다면?!
    
    완수된 순간에 임무를 하라고 명령한 프로그래머에게 임무가 완수되었다고 알려주며 동시에 임무 완수 후 할일을 전달해주는 것이 바로 Callback 이다.

    따라서 우리는 Retrofit을 사용한 통신을 Callback으로 등록하여 서버에게 서비스를 요청하여 응답받는 작업을 다른 작업들과 병렬적으로 하고, 응답을 받으면 Callback 알려줘야 하는 것이다.

    그리고, Callback을 통해 알려주려면 먼저 Call 작업이 실행되어야 한다!

    ![30](https://user-images.githubusercontent.com/31889335/70029772-23c78e80-15eb-11ea-93b5-eba6b6cbdfe4.PNG)

    위 코드의 onFailure 코드를 아래와 같이 바꾸면 서버 통신에 실패했을 때 원인을 알 수 있다!

    ![31](https://user-images.githubusercontent.com/31889335/70053788-83d52980-1619-11ea-9b95-bcaa5d77f53c.PNG)

    그럼 이제 Call 객체를 생성할 때는 실제 구현체를 사용하고, 그 구현체의 기반이 되는 Interface에 정의해놓은 요청 메소드를 작성해주면 된다. 요청 메소드에 매개변수가 있는 경우에는 알맞은 매개변수 값을 여기서 넣어주면 된다!

    > 이 코드 전체는 서버와의 통신으로 응답이 필요한 곳에 작성하면 된다.
    >
    > 나는 리사이클러뷰의 item 데이터들을 서버에서 받아와야 했으므로 리사이클러뷰의 data를 선언해주는 부분에 위 코드를 삽입하였다.
    >
    > --> 이 프로젝트의 GitrepositoryActivity.kt 참고

    이렇게 Call 객체를 생성한 후, enqueue라는 메소드를 호출하여 Call의 비동기 작업이 끝났을 때 동작할 Callback 을 작성해주면 된다. 코드의 onResponse 함수 내의 TODO 자리에 Callback 을 등록하면 된다!

    <br>

    💻💻💻 이렇게까지 하면 통신 끝!!!!!!!! 어서 빌드 해보자!!!!!! 💻💻💻



