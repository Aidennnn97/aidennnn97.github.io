---
layout: post
title: Dev-JSPServlet-post6
description: >
  JSP/Servlet
sitemap: false
categories:
  - dev
  - jspservlet
---

# JSP

## 뷰 컴포넌트와 JSP

MVC 아키텍처에서 뷰 컴포넌트는 JSP기술로 만든다. 왜냐하면 화면만들기가 쉽기 때문이다.

JSP 사용 전

![그림](/assets/img/jspservlet/0603/0603-6.png){: width="500" height="500"}

JSP 사용 후

![그림](/assets/img/jspservlet/0603/0603-7.png){: width="500" height="500"}

## JSP 구동원리

![그림](/assets/img/jspservlet/0603/0603-8.png){: width="500" height="500"}

JSP를 사용하면 개발자는 자바로 출력문을 작성할 필요가 없다. JSP엔진이 자바 출력문을 만들기 때문에 웹 브라우저로 출력할 HTML을 작성하기가 매우 쉬워진다. 즉, JSP는 서블릿 자바 파일을 만들기 위한 템플릿으로 사용되어지는 것이다.

## JSP의 주요 구성요소

JSP를 구성하는 요소는 크게 두 가지로 나눌 수 있다. 템플릿 데이터와 JSP전용 태그이다.
템플릿 데이터는 클라이언트로 출력되는 콘텐츠로 예를 들면 HTML, JS, StyleSheet, JSON 문자열, XML, 일반 텍스트 등이 있고, JSP전용 태그는 특정 자바 명령문으로 바뀌는 태그이다.

### 템플릿 데이터

![그림](/assets/img/jspservlet/0603/0603-9.png){: width="500" height="500"}

### JSP 전용태그

- 지시자
  - 지시자나 속성에 따라 특별한 자바 코드를 생성한다.
~~~
<%@ 지시자 ... %>
<%@ page
    language = "java"
    contentType = "text/html;charset=UTF-8"
    pageEncoding = "UTF-8" %>
<%@ page import = "java.util.*, java.io.*" %>
~~~

- 스크립트릿
  - 서블릿 파일을 만들 때 그대로 복사된다.
~~~
<%
String v1 = "";
String v2 = "";
%>
~~~

- 선언문
  - 서블릿 클래스의 멤버(변수나 메서드)를 선언할 때 사용한다.
~~~
<%! 
    private String calculate(int a, int b, String op){
        int r = 0;
        ...

    }
%>
~~~

- 표현식
  - 문자열을 출력할 때 사용한다.
~~~
<%= 결과를 반환하는 자바 표현식 %>
<%=v1 %>
<%=selected[0] %>
~~~

- 액션태그
  - 자바빈 연결

~~~
<jsp:action> </jsp:action>
~~~

- 주석

~~~
<%-- --%>
~~~

## JSP 액션 태그의 사용

JSP 페이지를 작성할 때, 가능한 자바 코드의 삽입을 최소화하는 것이 유지 보수에 좋다. 이를 위해 JSP에서는 다양한 JSP전용 태그를 제공하고 있고 이 태그들의 집합을 'JSP액션' 이라 한다.
JSP액션을 사용하면 자바로 직접 코딩하는 것보다 빠르고 쉽게 원하는 기능을 작성할 수 있다.

### \<jsp:forward>

현재 페이지에서 다른 특정 페이지로 전환할 때 사용한다.
~~~
<jsp:forward page="hello.jsp"/>
~~~

### \<jsp:include>

현재 페이지에 다른 특정 페이지를 삽입할 때 사용한다.
~~~
<jsp:include page="hello.jsp"/>
~~~

### \<jsp:param>

forward 및 include 태그에 데이터 전달을 목적으로 사용되는 태그로, 이름과 값으로 이루어져 있고, jsp:forward, jsp:include, jsp:params의 자식 태그로 사용할 수 있다.
~~~
<jsp:forward page="hello.jsp"/>
    <jsp:param name="id" value="abcd"/>
    <jsp:param name="pw" value="1234"/>
</jsp:forward>
~~~



🔍 **참고자료**

엄진영 유튜브 : <https://www.youtube.com/c/%EC%97%84%EC%A7%84%EC%98%81>

자바웹개발워크북 : <https://book.naver.com/bookdb/book_detail.nhn?bid=7623127>

유튜브 : <https://www.youtube.com/c/WizcenterSeoul>