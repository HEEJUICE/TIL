# Web Service Architecture

웹 서비스 구조에는 이렇게 크게 3가지가 있다.

- Client (Web Browser)

- Server

- DB (이번 시간에서는 다루지 않음)

---

- Client(ajax): 유저와 상호작용을 담당한다. 클라이언트에서 http라는 프로토콜을 사용해서 서버 API에 요청을 한다.
- Server(API): 리소스 요청과 응답에 대한 처리를 한다.
- DB: 리소스를 저장한다.

---

## 그럼 Web Service Architecture는 어떻게 구성되어 있을까? (Elements of Web Architecture)

- Browser
- Server
- API
- HTTP
- Ajax

---

### 1. Browser & Server

웹브라우저와 웹서버는 서로 연결되어 있기 때문에 같이 다루겠다.

웹서버가 하는 일은 어떤 정보를 저장하고 있다가 웹브라우저의 요청(Request)에 따라 그 정보를 웹브라우저에게 응답(Response)하는 역할을 하는데 이 정보라 함은 HTML, CSS, JS이다. 웹서버는 이 코드들을 브라우저에게 전달하게 되고 브라우저는 이 코드들을 수신, 해석(Interpreting)하여 사람들이 쉽게 알아볼 수 있는 형태로 display하는 역할을 한다.

즉, 브라우저는 사용자가 서버에게 요청한 정보를 브라우저를 통해서 대신 물어봐주는 역할을 하는 것이다. 그럼 서버는 서버가 갖고 있는 정보를 브라우저에게 보내주면서 응답 헤더(Response Header)를 만들어주는 역할을 한다. 그럼 다시 브라우저는 이 응답헤더를 가지고 적당히 사용자가 볼 수 있게 브라우저에 띄워주는 역할을 한다.

### 2. API

API는 Application Programming Interface의 줄임말이다.

사전적 정의로는 _“**API**(Application Programming Interface, 응용 프로그램 프로그래밍 인터페이스)는 응용 프로그램에서 사용할 수 있도록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다.”_ 이다.

먼저, API를 알기 전에 UI를 짚고 넘어가보자.
UI는 User Interface의 줄임말로서 interface란 용어가 공통적으로 들어간다.
UI가 사용자와 사용자가 다룰 대상(하드웨어 혹은 소프트웨어)을 연결한다면 API는 프로그램과 또 다른 프로그램을 연결해주는 역할을 한다고 볼 수 있겠다.

예를 들어서 지도어플, 카카오페이를 사용한다고 했을 때, 단순히 지도의 데이터 또는 결제하는 시스템의 데이터(코드 파일)만 제공된다면 사용자들은 쉽게 이 데이터들을 사용할 수 없을 것이다. 이 데이터들을 사용자들이 손 쉽게 다룰 수 있는 어플이라는 매개체를 만드는 것이 API이다.

### 3. HTTP

HTTP는 HyperText Transfer Protocal의 줄임말이다.
HTTP는 웹브라우저와 웹서버가 서로 request와 response 하는 것을 나타낸다. 웹브라우저와 웹서버가 HTML, CSS, JS의 파일들을 서로 주고받을 때 서로가 알아들을 수 있는 공통언어가 필요하다. 이 공통언어가 바로 HTTP이다.

이 HTTP 메시지들을 보려면 Chrome Browser 기준, 개발자 도구창을 열어서 'Network'탭을 클릭하면 웹브라우저와 웹서버가 주고받은 데이터, HTTP 메시지들을 볼 수 있다.

HTTP 요청과 응답은 둘 다 header와 body를 가지며 header에는 '어디서 보내는 요청인가 (origin)', '컨텐츠 타입은 무엇인가 (content-type)', '어떤 클라이언트를 이용해 보냈는가 (user-agent)' 를 볼 수 있다. body는 모든 http method가 가지는 것은 아니며 body는 서버에 데이터를 보내기 위한 공간으로 사용된다.

HTTP는 Stateless 속성과 Connectionless 속성을 가진다.

Stateless란, http의 요청은 모두 독립적이다. 때문에 한 번 준 요청에 대해서 응답을 한 번 받았으면 이어서 요청을 다시 할 수 없다.
ex) 삼겹살 먹을래? -> 그래, 먹자! (요청 끝) 그럼 지금 먹으러 갈래? -> 뭐를? (전의 삼겹살 요청 준 것은 없어짐)

Connectionless란, 한 번의 요청에는 한 번의 응답을 한다. 고로 응답 이후에는 연결이 끊긴다.
ex) 밥 먹으러 갈래? -> 그래, 먹자! (응답 끝) 뭐 먹으러 갈거야? (재응답 x)

HTTP method는 4가지가 있다.

- Get - 서버에 자원을 요청
- Post - 서버에 자원을 생성
- Put - 서버의 자원을 수정
- Delete - 서버의 자원을 제거

### 4. Ajax

![](https://images.velog.io/images/heejuice/post/eff2bea8-ce68-4055-8348-e25bd184faef/image.png)
Ajax를 활용한 '비동기식' 모델
![](https://images.velog.io/images/heejuice/post/9df5cad6-ee66-4349-9d91-390168d318ab/image.png)
기존 '동기식' 모델

![](https://images.velog.io/images/heejuice/post/dd039690-83e8-494f-a16d-fcaa5b7b682b/image.png)

![](https://images.velog.io/images/heejuice/post/85a094f3-49ef-43b5-90f3-1f331305ed46/image.png)
비동기식
![](https://images.velog.io/images/heejuice/post/064fddd4-6da2-4fc7-9630-846766aba63b/image.png)
동기식

Ajax는 Asynchronous JavaScript and XML 의 줄임말이다.
Ajax는 기존에 사용되던 여러 기술을 함께 사용하여, 웹 페이지의 일부분만을 갱신할 수 있도록 해주는 개발 기법이다. 이러한 Ajax에서 사용하는 기존 기술은 다음과 같다.

- 웹 페이지의 표현을 위한 HTML과 CSS
- 데이터에 접근하거나 화면 구성을 동적으로 조작하기 위해 사용되는 DOM 모델
- 데이터의 교환을 위한 JSON이나 XML
- 웹 서버와의 비동기식 통신을 위한 XMLHttpRequest 객체
- 위에서 언급한 모든 기술을 결합하여 사용자의 작업 흐름을 제어하는 데 사용되는 자바스크립트

Ajax를 이용한 웹 응용 프로그램은 자바스크립트 코드를 통해 웹 서버와 통신을 하게 된다.
따라서 사용자의 동작에는 영향을 주지 않으면서도 백그라운드에서 지속해서 서버와 통신할 수 있다.

**<Ajax를 이용한 웹 응용 프로그램의 동작 원리>**

1. 사용자에 의한 요청 이벤트가 발생한다.
2. 요청 이벤트가 발생하면 이벤트 핸들러에 의해 자바스크립트가 호출된다.
3. 자바스크립트는 XMLHttpRequest 객체를 사용하여 서버로 요청을 보낸다.
4. 이때 웹 브라우저는 요청을 보내고 나서, 서버의 응답을 기다릴 필요 없이 다른 작업을 처리할 수 있다.
5. 서버는 전달받은 XMLHttpRequest 객체를 가지고 Ajax 요청을 처리한다.
6. 서버는 처리한 결과를 HTML, XML 또는 JSON 형태의 데이터로 웹 브라우저에 전달한다.
7. 이때 전달되는 응답은 새로운 페이지를 전부 보내는 것이 아니라 필요한 데이터만을 전달한다.
8. 서버로부터 전달받은 데이터를 가지고 웹 페이지의 일부분만을 갱신하는 자바스크립트를 호출한다.
9. 결과적으로 웹 페이지의 일부분만이 다시 로딩되어 표시된다.

**<Ajax의 장점>**

- 웹 페이지 전체를 다시 로딩하지 않고도, 웹 페이지의 일부분만을 갱신 가능
- 웹 페이지가 로드된 후에 서버로 데이터 요청을 보낼 수 있음
- 웹 페이지가 로드된 후에 서버로부터 데이터를 받을 수 있음
- 백그라운드 영역에서 서버로 데이터를 보낼 수 있음

**<XMLHttpRequest 사용>**

```js
var xhr = new XMLHttpRequest();
xhr.open("get", "http://52.78.213.9:3000/messages");

// 요청의 상태 변화를 추적합니다
xhr.onreadystatechange = function() {
  if (xhr.readyState !== 4) return;
  // readyState 4: 완료

  if (xhr.status === 200) {
    // status 200: 성공
    console.log(xhr.responseText); // 서버로부터 온 응답
  } else {
    console.log("에러: " + xhr.status); // 요청 도중 에러 발생
  }
};

xhr.send(); // 요청 전송
```

**<XMLHttpRequest jQuery 사용>**

```js
$.get("http://52.78.213.9:3000/messages", function(response) {
  // response: 서버로부터 온 응답
});
```

**<Fetch API 사용>**

```js
fetch("http://52.78.213.9:3000/messages")
  .then(function(response) {
    return response.json();
  })
  .then(function(json) {
    // json 형태로 전달받은 서버로부터의 응답
  });
```

**<Fetch API와 XMLHttpRequest의 차이>**

**Fetch**

- 문서를 소비하는 기본 제공 방법이 없다.
- 아직 시간 초과에 있어서 설정할 방법이 없다.
- 응답 헤더 content-type을 덮어 씌울 수 없다.
- 만약 content-length 응답 헤더가 현재 있어도 아직 노출되지 않은 상태라면, 그 body의 길이를 스트리밍 하는 동안 알 수 없다.
- 설령 요청이 이미 완료된 경우에도 신호의 중단 핸들러를 호출한다.

**XHR**

- 쿠키를 보내지 _않을_ 방법이 없다. (비표준 mozAnon 플래그나 AnonXMLHttpRequest 생성자를 사용하는 것 제외)
- *FormData*인스턴스들을 리턴할 수 없다.
- 'Fetch'의 'no-cors'와 상응하는 기능이 없다.
- 항상 redirect가 뒤따른다.

내가 모르는 차이도 있어서 출처를 첨부한다.(https://stackoverflow.com/questions/35549547/fetch-api-vs-xmlhttprequest)
