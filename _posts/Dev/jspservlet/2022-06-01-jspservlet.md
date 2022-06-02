---
layout: post
title: Dev-JSPServlet-post3
description: >
  JSP/Servlet
sitemap: false
categories:
  - dev
  - jspservlet
---

# 서블릿 프로그래밍

## CGI

![그림](/assets/img/jspservlet/0601/0601-1.png){: width="500" height="500"}

웹 서버와 프로그램 사이의 데이터를 주고받는 규칙을 **CGI(Common Gateway Interface)** 라고 하고, 웹 서버에 의해 실행되며 CGI규칙에 따라서 웹 서버와 데이터를 주고 받도록 작성된 프로그램을 'CGI 프로그램'이라고 한다.

✏️ 컴파일 방식의 CGI프로그램

![그림](/assets/img/jspservlet/0601/0601-2.png){: width="500" height="500"}

웹 서버가 클라이언트로부터 요청을 받으면 애플리케이션을 바로실행 한다.

✏️ 컴파일 방식의 CGI프로그램

![그림](/assets/img/jspservlet/0601/0601-3.png){: width="500" height="500"}

인터프리터 방식은 기계어로 되어있는게 아니라 소스 그 자체로 되어있다. 서버에서 스크립트를 직접 실행할 수 없기 때문에 중간에서 스크립트엔진의 도움이 필요하다. 이 스크립트엔진을 바로 인터프리터라고 부르는 것이다.

## 서블릿

![그림](/assets/img/jspservlet/0601/0601-4.png){: width="500" height="500"}

자바 CGI프로그램은 C/C++ 처럼 컴파일 방식이다. 자바로 만든 CGI프로그램을 **서블릿** 이라고 부른다. 자바 서블릿이 다른 CGI프로그램과 다른 점은, 웹 서버와 직접 데이터를 주고받지 않으며, 전문 프로그램에 의해 관리된다는 것이다.

## GenericServlet의 사용

✏️ 인터페이스란?
    - 호출자와 피호출자 사이의 호출규칙을 의미한다.

![그림](/assets/img/jspservlet/0601/0601-5.png){: width="500" height="500"}

- init() : 인스턴스 생성 후 딱 한 번 호출되는 메서드.
- destroy() : 서버 또는 웹 애플리케이션 종료 직전 호출되는 메서드.
- service() : 클라이언트 요청 때 마다 호출되는 메서드.

✏️ 서블릿 클래스란?

- 서블릿 인터페이스를 구현한 자바프로그램을 말한다.

추상클래스는 서브클래스의 공통적인 속성이나 메서드를 상속해주는 용도로 클래스를 정의하고자 할때 정의한다.

![그림](/assets/img/jspservlet/0601/0601-6.png){: width="500" height="500"}

'인터페이스를 구현하는 클래스는 반드시 인터페이스에 선언된 모든 메서드를 구현해야 한다.' 라는 것이 자바의 법이기 때문에 사용되지 않는 나머지 메서드에 대해서 빈 메서드라도 구현해야한다. 이런 불편한 점을 해소하기 위해 등장한 것이 GenericServlet 추상 클래스이다.

## ServletRequest

- getParameter() \- GET이나 POST요청으로 들어온 매개변수 값을 꺼낼 때 사용.
- getParameterNames \- 요청정보에서 매개변수 이름만 추출하여 반환.
- getParameterValues \- 요청정보에서 매개변수 값만 추출하여 반환.
- setCharacterEncoding() \- POST요청의 매개변수에 대해 문자 집합을 설정.

## ServletResponse

- setContentType() \- 출력할 데이터의 인코딩 형식과 문자 집합을 지정.
- setCharacterEncoding() \- 출력할 데이터의 문자 집합을 지정.
- getWriter() \- 클라이언트로 출력할 수 있도록 출력 스트림 객체를 반환.

## @WebServlet Annotation

@WebServlet 어노테이션을 사용하면 서블릿의 배치정보를 작성할 수 있다.
- name \- 서블릿의 이름을 설정하는 속성. 기본값은 빈문자열("")이다.
 > @WebServlet(name = "서블릿이름")

- urlPatterns \- 서블릿의 URL목록을 설정하는 속성. 속성값으로 String배열이 오고 기본 값은 빈 배열({})이다.
  > @WebServlet(urlPatterns = "/hello")

  > @WebServlet(urlPatterns = {"/hello"})

  >@WebServlet(urlPatterns = {"/hello", "helo.do", "hello.action"})

- value \- urlPatterns와 같은 용도. 어노테이션의 문법에서는 속성이름이 'value'인 경우는 속성 이름 없이 값만 설정할 수 있다.
  > @WebServlet(value = "/hello")

  > @WebServlet("/hello")

  > @WebServlet(value = "/hello", name = "helloworld")

## 정리

웹 서버와 웹 애플리케이션 사이에는 데이터를 주고받기 위한 규칙이 있는데 이것을 **CGI** 라고 하며 보통 웹 애플리케이션을 **CGI프로그램** 이라고도 부른다.
특히 자바로 만든 웹 애플리케이션을 서블릿이라고 부르고, 클라이언트에게 서비스를 제공하는 작은 단위의 서버 프로그램이라는 뜻이다.
서블릿 컨테이너는 서블릿의 생성, 실행, 소멸까지 서블릿의 생명주기를 관리하는 프로그램이다.
클라이언트로부터 요청이 들어오면 서블릿 컨테이너는 javax.servlet.Servlet 인터페이스에 정의되어 있는 호출규칙에 따라 서블릿의 메서드를 호출한다. 따라서 서블릿을 만들 때는 반드시 Servlet 인터페이스를 구현해야만 한다.
GenericServlet 추상클래스는 Servlet 인터페이스에 선언된 메서드 중 service()를 제외한 나머지 메서드를 모두 구현해 놓았다. 따라서 서블릿을 만들 때 Servlet 인터페이스를 직접 구현하는 것 보다 GenericServlet 클래스를 상속받으면 service() 구현에만 집중하면 되기 때문에 좀 더 쉽고 편리하게 개발할 수 있다.

🔍 **참고자료**

엄진영 유튜브 : <https://www.youtube.com/c/%EC%97%84%EC%A7%84%EC%98%81>

자바웹개발워크북 : <https://book.naver.com/bookdb/book_detail.nhn?bid=7623127>