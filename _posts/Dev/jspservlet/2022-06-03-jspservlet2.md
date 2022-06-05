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

## JSP 내장 객체

JSP 페이지에서 스크립트릿 <% %> 이나 표현식 <%= %>을 작성할 때 별도의 선언 없이 사용하는 자바 객체가 있다. 이런 객체를 JSP 내장 객체라고 한다. JSP에서 제공되는 내장객체는 JSP컨테이너에 의해 Servlet으로 변화될 때 자동으로 객체가 생성된다.

### 내장 객체 종류
- 입출력 객체: request, response, out
- 서블릿 객체: page, config
- 세션 객체: session
- 예외 객체: exception
- pageContext, application

## 자바 빈

반복적인 작업을 효율저거으로 하기 위해 빈을 사용한다. 빈이란? JAVA언어의 데이터(속성)와 기능(메서드)으로 이루어진 클래스이다. jsp페이지를 만들고, 액션태그를 이용하여 빈을 사용 하고 빈의 내부 데이터를 처리한다.

~~~
public class Student{
  private String name;
  private int age, grade, studentNum;
}
----------------------------------------
public String getName(){
  return name;
}
public void setName(String name){
  this.name = name;
}
...
~~~

데이터 객체에는 데이터가 있어 그에 해당하는 getter, setter가 있다. 빈을 만든다는 것은 데이터 객체를 만들기 위한 클래스를 만드는 것이다.

### JSP 액션 태그 - \<jsp:useBean>

\<jsp:useBean> 액션 태그는 application, session, request, page 보관소에 저장된 자바 객체를 꺼낼 수 있다. 만약 보관소에 저장된 객체가 없다면, 새로 생성하여 해당 보관소에 저장한다.

~~~
// jsp:useBean 문법
<jsp:useBean id = "이름" class = "클래스명" type = "타입명" scope = "page|request|session|application">

scope
//page: 생성된 페이지 내에서만 사용 가능
//request: 요청된 페이지 내에서만 사용 가능
//session: 웹 브라우저의 생명주기와 동일하게 사용 가능
//application: 웹 어플리케이션 생명주기와 동일하게 사용 가능
~~~

~~~
//데이터 값을 설정 할 때 사용
<jsp:setProperty name = "student" property = "name" value = "홍길동"/>
~~~

~~~
//데이터 값을 가져올 때 사용
<jsp:getProperty name = "student" property = "name" />
~~~

## EL(Expression Language)

EL(Expression Language)은 콤마(,)와 대괄호([])를 사용하여 자바 빈의 프로퍼티나 맵, 리스트, 배열의 값을 보다 쉽게 꺼내게 해주는 기술이다. 또한, 스태틱으로 선언된 메서드를 호출 할 수도 있다. EL을 사용하면 액션 태그를 사용하는 것보다 훨씬 더 간단히 보관소에 들어있는 객체에 접근하여 값을 꺼내거나 메서드를 호출할 수 있다.

### EL표기법

EL은 ${}와 #{}을 사용하여 값을 표현한다.

~~~
//   표현식             EL
<%= value %>  -->  ${value}
~~~

EL도 \<jsp:useBean> 처럼 네 군데 보관소에서 값을 꺼낼 수 있다. 다만 다른 점은 EL로는 객체를 생성할 수 없다는 것이다.

~~~
pageScope   ->    JspContext
requestScope    ->    ServletRequest
sessionScope    ->    HttpSession
applicationScope    ->    ServletContext

${requestScope.member.no}
~~~

### EL 내장 객체

> pageScope, requestScope, sessionScope, applicationScope
> 
> param, paramValues, initParam, cookie 등

## JSTL(JSP standard Tag Library)

JSTL태그를 이용하면 JSP페이지에서 자바 코딩을 줄일 수 있다.
JSTL은 JSP의 기본 태그가 아니므로 사용하려면 JSTL API 및 이를 구현한 자바 라이브러리를 별도로 내려받아야 한다.

### JSTL 라이브러리 준비

[mavenrepository.com](https://mvnrepository.com/artifact/javax.servlet/jstl/1.2) 에서 jstl을 검색하고 jar파일을 다운받고, jar파일을 프로젝트의 webapp/WEB-INF/lib 폴더에 넣어준다.

### JSTL 주요 태그의 사용법

JSTL 확장 태그를 사용하려면 그 태그의 라이브러리를 선언해야한다.

> <%@ taglib uri = "사용할 태그의 라이브러리 URI" prefix = "접두사" %>

JSTL은 다섯가지의 라이브러리를 제공 한다.(Core, XML Processing, l18N formatting, DAtabase, Functions)

| 태그 라이브러리    | 접두사       | 네임스페이스의 URI 식별자                  |
| -------------- | ---------  | ------------------------------------- |
| Core           | c          | http://java.sun.com/jsp/jstl/core     |
| XML            | x          | http://java.sun.com/jsp/jstl/xml      |
| l18N           | fmt        | http://java.sun.com/jsp/jstl/fmt      |
| Database       | sql        | http://java.sun.com/jsp/jstl/sql      |
| Functions      | fn         | http://java.sun.com/jsp/jstl/functions|

~~~
<%@ taglib uri = "http://java.sun.com/jsp/jstl/core" prefix = "c" %>

<c:out value = "출력값" default = "기본값" />

<c:set var = "변수명" value = "설정값" target = "객체" property = "값" scope = "범위" />

<c:remove var = "변수명" scope = "범위" />

<c:catch var = "변수명" />

<c:if test = "조건" var = "조건 처리 변수명" scope = "범위" />

<c:choose>
  <c:when test = "조건"> 처리내용 </c:when>
  <c:otherwise> 처리내용 </c:otherwise>
</c:choose>

<c:forEach var = "변수명" items = "객체명" begin = "시작인덱스" end = "종료인덱스" step = "증감식" varStatus = "상태변수" > 콘텐츠 </c:forEach>

<c:redirect url = "url">

<c:param name = "파라미터명" value = "값">
~~~

🔍 **참고자료**

엄진영 유튜브 : <https://www.youtube.com/c/%EC%97%84%EC%A7%84%EC%98%81>

자바웹개발워크북 : <https://book.naver.com/bookdb/book_detail.nhn?bid=7623127>

유튜브 : <https://www.youtube.com/c/WizcenterSeoul>