# Servlet
> 클라이언트의 요청을 처리하고, 그 결과를 반환하는 
 Servlet 클래스의 구현 규칙을 지킨 자바 웹 프로그래밍 기술

클라이언트가 어떠한 요청을 하면 그에 대한 결과를 다시 전송해주어야 하는데, 이러한 역할을 하는 자바 프로그램이다. 
어떠한 사용자가 로그인을 하려고 할 때. 사용자는 아이디와 비밀번호를 입력하고, 로그인 버튼을 누른다.
그때 서버는 클라이언트의 아이디와 비밀번호를 확인하고, 다음 페이지를 띄워주어야 하는데, 이러한 역할을 수행하는 것이 바로 서블릿(Servlet)이며 그래서 서블릿은 자바로 구현 된 *CGI라고 흔히 말한다.

## CGI란?

일반적으로 웹서버는 정적인 페이지만을 제공한다. 그렇기 때문에 동적인 페이지를 제공하기 위해서 웹서버는 다른 곳에 도움을 요청하여 동적인 페이지를 작성해야 한다. 동적인 페이지로는 임의의 이미지만을 보여주는 페이지와 같이
사용자가 요청한 시점에 페이지를 생성해서 전달해 주는 것을 의미한다. 여기서 웹서버가 동적인 페이지를 제공할 수 있도록 도와주는 어플리케이션이 서블릿이며, 동적인 페이지를 생성하는 어플리케이션이 <Strong>CGI이다. 

## [ Servlet 특징 ]

1. 클라이언트의 요청에 대해 동적으로 작동하는 웹 어플리케이션 컴포넌트

2. html을 사용하여 요청에 응답한다.

3. Java Thread를 이용하여 동작한다.

4. MVC 패턴에서 Controller로 이용된다.

5. HTTP 프로토콜 서비스를 지원하는 javax.servlet.http.HttpServlet 클래스를 상속받는다.

6. UDP보다 처리 속도가 느리다.

7. HTML 변경 시 Servlet을 재컴파일해야 하는 단점이 있다.

## [ Servlet 동작 방식 ]

사용자(클라이언트)가 URL을 입력하면 HTTP Request가 Servlet Container로 전송합니다.
요청을 전송받은 Servlet Container는 HttpServletRequest, HttpServletResponse 객체를 생성합니다.
web.xml을 기반으로 사용자가 요청한 URL이 어느 서블릿에 대한 요청인지 찾습니다.
해당 서블릿에서 service메소드를 호출한 후 클리아언트의 GET, POST여부에 따라 doGet() 또는 doPost()를 호출합니다.
doGet() or doPost() 메소드는 동적 페이지를 생성한 후 HttpServletResponse객체에 응답을 보냅니다.
응답이 끝나면 HttpServletRequest, HttpServletResponse 두 객체를 소멸시킵니다.

# Servlet Container
+ 우리가 서버에 서블릿을 만들었다고 해서 스스로 작동하는 것이 아니고 서블릿을 관리해주는 것이 필요한데, 그러한 역할을 하는 것이 바로 서블릿 컨테이너 입니다. 예를 들어, 서블릿이 어떠한 역할을 수행하는 정의서라고 보면, 서블릿 컨테이너는 그 정의서를 보고 수행한다고 볼 수 있습니다. 서블릿 컨테이너는 클라이언트의 요청(Request)을 받아주고 응답(Response)할 수 있게, 웹서버와 소켓으로 통신하며 대표적인 예로 톰캣(Tomcat)이 있습니다. 톰캣은 실제로 웹 서버와 통신하여 JSP(자바 서버 페이지)와 Servlet이 작동하는 환경을 제공해줍니다.

## [Servlet Container 역할]
  1. 웹서버와의 통신 지원
서블릿 컨테이너는 서블릿과 웹서버가 손쉽게 통신할 수 있게 해줍니다. 일반적으로 우리는 소켓을 만들고 listen, 
accept 등을 해야하지만 서블릿 컨테이너는 이러한 기능을 API로 제공하여 복잡한 과정을 생략할 수 있게 해줍니다.
그래서 개발자가 서블릿에 구현해야 할 비지니스 로직에 대해서만 초점을 두게끔 도와줍니다.

  2. 서블릿 생명주기(Life Cycle) 관리 
서블릿 컨테이너는 서블릿의 탄생과 죽음을 관리합니다. 서블릿 클래스를 로딩하여 인스턴스화하고, 
초기화 메소드를 호출하고, 요청이 들어오면 적절한 서블릿 메소드를 호출합니다. 
또한 서블릿이 생명을 다 한 순간에는 적절하게 Garbage Collection(가비지 컬렉션)을 진행하여 편의를 제공합니다.


  3. 멀티쓰레드 지원 및 관리 
서블릿 컨테이너는 요청이 올 때 마다 새로운 자바 쓰레드를 하나 생성하는데, HTTP 서비스 메소드를
실행하고 나면, 쓰레드는 자동으로 죽게됩니다. 원래는 쓰레드를 관리해야 하지만 서버가 다중 쓰레드를
생성 및 운영해주니 쓰레드의 안정성에 대해서 걱정하지 않아도 됩니다.


  4. 선언적인 보안 관리 
서블릿 컨테이너를 사용하면 개발자는 보안에 관련된 내용을 서블릿 또는 자바 클래스에 구현해 놓지 않아도 됩니다.
일반적으로 보안관리는 XML 배포 서술자에 다가 기록하므로, 보안에 대해 수정할 일이 생겨도 자바 소스 코드를 
수정하여 다시 컴파일 하지 않아도 보안관리가 가능합니다.

> 출처: https://mangkyu.tistory.com/14 [MangKyu's Diary]

# HttpServlet

+ WAS가 웹브라우저로부터 밭은 서블릿 요청을 받으면
  1. 요청을 받을 때 전달 받은 정보를 HttpServletRequest객체를 생성하여 저장
  2. 웹브라우져에게 응답을 돌려줄 HttpServletResponse객체를 생성(빈 객체)
  3. 생성된 HttpServletRequest(정보가 저장된)와 HttpServletResponse(비어 있는)를 Servlet에게 전달

## HttpServletRequest

+ Http프로토콜의 request 정보를 서블릿에게 전달하기 위한 목적으로 사용
+ Header정보, Parameter, Cookie, URI, URL 등의 정보를 읽어들이는 메소드를 가진 클래스
+ Body의 Stream을 읽어들이는 메소드를 가지고 있음

## HttpServletResponse
+ Servlet은 HttpServletResponse객체에 Content Type, 응답코드, 응답 메시지등을 담아서 전송함
# 
## URL(Uniform Resource Locator)과 URI(Uniform Resource Identifier)

+ URL(Uniform Resource Locator) : 실제 파일의 위치
ex) http://risingJ/src/resource/asd.html (직접적으로 파일의 경로를 보여줌)

+ URI(Uniform Resource Identifier) : 파일의 위치를 알 수 있는 식별자
ex) http://risingJ/asd (rewrite 기술을 사용하여 만든 의미있는 식별자)
-> request 정보안에 담겨있음