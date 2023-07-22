## **HTTP Protocol**
HTTP(Hypertext Transfer Protocol)는 인터넷상에서 데이터를 주고 받기 위한 서버/클라이언트 모델을 따르는 프로토콜이다. 애플리케이션 레벨의 프로토콜로 TCP/IP위에서 작동한다.

> ### **서버/클라이언트 모델이란?**
> 서비스 제공자(service provider)와 서비스 요청자(service requester)로 구분되는 네트워크 모델이다. 서비스 제공자의 역할을 하는 측을 서버, 서비스 요청자의 역할을 하는 측을 클라이언트라고 한다. 서버/클라이언트 모델에서 모든 자원은 서버에 집중된다. 클라이언트는 데이터 프리젠테이션(재현)을 위한 최소한의 자원을 가지는게 일반적이다.

> ### **TCP/IP 프로토콜이란?**
> 인터넷 프로토콜 중 가장 중요한 역할을 하는 TCP와 IP의 합성어로 인터넷 동작의 중심이 되는 통신 규약으로 데이터의 흐름 관리, 데이터의 정확성 확인(TCP 역할), 패킷을 목적지까지 전송하는 역할(IP 역할)을 담당한다. IP는 데이터를 한 장소에서 다른 장소로 정확하게 옮겨주는 역할을 하며, TCP는 전체 데이터가 잘 전송될 수 있도록 데이터의 흐름을 조절하고 성공적으로 상대편 컴퓨터에 도착할 수 있도록 보장해주는 역할을 한다.

## **웹브라우저의 통신 과정**
![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99E63F4B5B6FCA4A01)

클라이언트에서 요청(request)를 보내면 서버는 요청을 처리해서 응답(response)한다.

+ ### **URL 분석 및 접속**
    웹 브라우저는 URL을 분석해 서버의 IP 주소와 포트(기본은 80)를 이용해 서버와 TCP/IP 연결을 요청한다.

+ ### **Request 헤더 전송**
    브라우저에서 요청 파일명 등이 기술된 헤더를 전송한다.

+ ### **Request 바디 전송**
    필요한 경우에, 로그인 폼에 입력한 데이터나 첨부 파일 등의 추가적인 데이터를 전송한다.

+ ### **Response 헤더 해석**
    서버에서 헤더를 수신하고 응답 상태(404 등)를 확인하며, 바디의 Content-Type 등을 확인한다.

+ ### **Response 바디 해석**
    바디가 있는 경우에, 서버에서 수신한 바디를 헤더에 기술된 Content-Type에 따라서, text/html인 경우에 HTML을 렌더링하고, image/jpeg인 경우에는 그림을 띄우는 등 적절히 해석한다.

## **Connectless & Stateless**
HTTP는 Connectless 방식으로 작동한다. 서버에 연결하고, 요청해서 응답을 받으면 연결을 끊어버린다. 기본적으로는 자원 하나에 대해서 하나의 연결을 만든다.

+ 장점 : 불특정 다수를 대상으로 하는 서비스에 적합한 방식이다. 수십만명이 웹 서비스를 사용하더라도 접속유지는 최소한으로 할 수 있기 때문에, 더 많은 유저의 요청을 처리할 수 있다.

+ 단점 : 연결을 끊어버리기 때문에, 클라이언트의 이전 상태를 알 수가 없다. 이러한 HTTP의 특징을 stateless라고 하는데, Connectless로 부터 파생되는 특징이라고 할 수 있다. 클라이언트의 이전 상태 정보를 알 수 없게 된다면, 웹 서비스를 하는데 당장에 문제가 생긴다. 클라이언트가 과거에 로그인을 성공하더라도 로그 정보를 유지할 수가 없다. HTTP는 cookie를 이용해서 이 문제를 해결하고 있다.

## **URI**
클라이언트 프로그램(웹 브라우저)은 URI를 이용하여 자원의 위치를 찾는다. URI는 HTTP와는 독립된 다른 체계다. HTTP는 전송 프로토콜이고, URI는 자원의 위치를 알려주기 위한 프로토콜이다. Uniform Resource Identifiers 의 줄임말로, World Wide Web 상에서 접근하고자 하는 자원의 위치를 나타내기 위해 사용한다. 자원은 문서, 이미지, 동영상, 프로그램, 이메일 등 모든 것이 될 수 있다.

> http://www.example.co.kr/index.html
> 1. http : 자원에 접근하기 위해서 http 프로토콜을 사용한다.
> 2. www.example.co.kr, 도메인 주소 : 자원의 인터넷 상에서의 위치이다. 도메인은 ip 주소로 변환되므로 ip 주소로 서버의 위치를 찾을 수 있다.
> 3. index.html : 요청할 자원의 이름이다.

이렇게 프로토콜, 위치, 자원명으로 (인터넷 상에서) 어디에 있던지 자원에 접근할 수 있다.

## **Method**
메서드는 요청의 종류를 서버에게 알려주기 위해서 사용한다.
+ ### **GET**
    주어진 URL에서 자원을 요청
+ ### **POST**
    주어진 URL로 자원의 생성을 요청
+ ### **PUT**
    주어진 URL로 자원의 대체를 요청
+ ### **DELETE**
    주어진 URL로 자원의 삭제를 요청
+ ### **HEAD**
    주어진 URL에서 자원의 헤더만을 요청, 해당 자원이 존재하는지 혹은 서버에 문제가 없는지를 확인하기 위해서 사용한다.
+ ### **OPTIONS**
    주어진 URL에서 처리 가능한 메소드의 목록을 요청

## **HTTP Request**
웹 브라우저는 웹 서버에 데이터를 요청하는 클라이언트 프로그램이다. 요청은 서버가 인식할 수 있는 약속된 형식(HTTP 형식)을 따라야 한다. 요청 데이터는 HEADER와 BODY로 구성된다.
![image](https://velog.velcdn.com/images%2Fdnjscksdn98%2Fpost%2F319733fc-8fcb-48d3-8880-7932485162ee%2Fhttp_request.png)

필수 요소로 요청의 제일 처음에 와야 하는 3가지 필드가 있다.
+ 요청 메서드 : GET, PUT, POST, PUSH, OPTIONS 등의 요청 방식이 온다.
+ 요청 URI : 요청하는 자원의 위치를 명시한다.
+ HTTP 프로토콜 버전 : 웹 브라우저가 사용하는 프로토콜 버전이다.

그 외의 키값
+ Host : 요청을 보내는 Host   
    ex) www.google.co.kr
+ Content-Type : 요청에 바디가 있는 경우 그 파일 포맷   
    ex) Content-Type : application/json
+ Cookie : 웹 브라우저에 저장된 쿠키들
+ User-Agent : 클라이언트의 정보, 이를 통해 사용하는 브라우저 감지

## **HTTP Response**
![image](https://velog.velcdn.com/images%2Fdnjscksdn98%2Fpost%2F42caeb0f-83f0-41e3-bfc7-ad169dbed518%2Fhttp_response.png)

+ 프로토콜과 응답코드 : 웹 브라우저가 사용하는 프로토콜, 서버의 응답 상태(1xx~5xx), 응답 메세지를 보여준다
+ Set-Cookie : 웹 브라우저에게 쿠키 생성을 요청   
    ex) Set-Cookie : UserID=tester; Max-Age=3600; Version=1
+ Content-Type : 응답에 바디가 있는 경우 그 파일 포맷   
    ex) Content-Type : text/html; charset=utf-8

## **HTTP Status Code**

### **2xx(성공)**
+ 200(성공) : 서버가 요청을 제대로 처리함
+ 201(생성됨) : 요청이 성공했으며, 새로운 리소스가 생성됨
+ 202(허용됨) : 요청을 받았으나 아직 처리하지 않음
+ 204(컨텐츠 없음) : 요청을 처리했지만, 컨텐츠를 제공하지 않음
+ 205(컨텐츠 재설정) : 요청을 처리했지만, 컨텐츠를 표시하지 않음. 그리고 문서를 재설정할 것을 요구함
+ 206(일부 성공) : 요청의 일부만 성공적으로 처리함

### **4xx(요청 오류)**
+ 400(잘못된 요청) : 서버가 요청의 구문을 인식하지 못함. 주로 헤더 포멧이 HTTP 규약에 맞지 않을 경우
+ 401(권한 없음) : 인증을 필요로 하는 요청, 인증 실패
+ 403(Forbidden, 금지됨) : 서버가 요청을 거부, 인가 실패
+ 404(Not Found, 찾을 수 없음) : 서버가 요청한 리소스를 찾을 수 없음
+ 405(허용하지 않는 방법) : 요청에 지정된 방법을 사용할 수 없다. 예를 들어 POST 방식으로 요청을 받는 서버에 GET 요청을 보내는 경우

### **5xx(서버 오류)**
+ 500(내부 서버 오류) : 서버에 오류가 발생하여 요청을 수행할 수 없다.