---
layout: post
title: Dev-JSPServlet-post7
description: >
  JSP/Servlet
sitemap: false
categories:
  - dev
  - jspservlet
---

# 데이터 보관소

데이터 보관소라는 것은  말 그대로 어떤 객체를 보관하는 보관소 역할을 한다.
보관소들은 이 보관소가 언제 생성되고 언제 소멸되는지 시점에 따라서 4가지를 서블릿에서 제공한다.

**ServletContext**, **HttpSession**, **ServletRequest**, **JspContext**

![그림](/assets/img/jspservlet/0603/0603-12.png){: width="500" height="500"}

- ServletContext 보관소
  - 웹 애플리케이션 당 1개만 시작 시 준비됨
  - 웹 애플리케이션이 실행되는 동안 유지되어야할 데이터(객체)가 있을때 보관
  - 웹 애플리케이션이 시작될 때 생성되어 종료될 때까지 유지
- HttpSession 보관소
  - 클라이언트의 최초 요청 시 생성되어 브라우저를 닫을 때까지 유지
  - 세션 ID를 통해 사용자 별 세션이 관리됨
- ServletRequest 보관소
  - 클라이언트의 요청이 들어올 때 생성되어, 클라이언트에게 응답 할 때까지 유지
  - 요청을 처리하는 동안 서블릿들끼리 공유할 데이터가 있을때 보관(포워딩, 인클루딩)

세션이 만들어지든 안만들어지든 요청이 들어오면 ServletRequest는 무조건 생성된다.

![그림](/assets/img/jspservlet/0603/0603-12.png){: width="500" height="500"}

- JspContext 보관소
  - JSP페이지를 실행하는 동안만 유지
  - 태그 핸들러와 데이터를 공유할 때 사용

## ServletContext의 활용

### ServletContext 활용 전

![그림](/assets/img/jspservlet/0603/0603-14.png){: width="500" height="500"}

서블릿에서 DBMS로부터 데이터를 가져오려면 서블릿마다 직접 Connection객체를 만들어야 했다.

### ServletContext 활용 후

![그림](/assets/img/jspservlet/0603/0603-15.png){: width="500" height="500"}

AppInitServlet이라는 서비스를 만들어 Connection을 미리 준비하게 만든다. 그러고나선 서블릿들이 커넥션객체가 필요하면 준비된 커넥션 객체를 가져다 쓴다. 이 때 커넥션객체는 당연히 ServletContext에 보관해 둔다.

![그림](/assets/img/jspservlet/0603/0603-16.png){: width="500" height="500"}

서블릿이라하면 클라이언트 요청이 들어와야지만이 실행하게된다. AppInitServlet처럼 서블릿들이 공통으로 사용할 자원을 준비하는 역할을하는 서블릿은 굳이 클라이언트가 요청하지 않아도 자동으로 실행할 수 있도록 하기 위해서 web.xml에 배치할 때 서블릿태그를 작성하게 된다. 단, 클라이언트의 요청을 처리하지 않기 때문에 URL을 매핑할 필요가 없어서 서블릿 매핑태그는 지워준다.

## HttepSession의 활용

세션은 쿠키와 마찬가지로 서버와의 관계를 유지하기 위한 수단이다. 단, 쿠키와 달리 클라이언트의 특정 위치에 저장되는 것이 아니라, 서버상에 객체로 존재한다. 따라서 세션은 서버에서만 접근이 가능하여 보안이 좋고, 저장할 수 있는 데이터에 한계가 없다.
세션은 클라이언트의 요청이 발생하면 자동생성 된다. 그리고 session이라는 내부 객체를 지원하여 세션의 속성을 설정 할 수 있다.

### HttpSession 준비

![그림](/assets/img/jspservlet/0603/0603-17.png){: width="500" height="500"}

### HttpSession에 데이터 보관

![그림](/assets/img/jspservlet/0603/0603-18.png){: width="500" height="500"}
~~~
Member member = new Member()
                    .setEmail(rs.getString("EMAIL"))
                    .setName(rs.getString("MNAME"));
HttpSession session = request.getSession();
session.setAttribute("member", member);
~~~

### HttpSession에 보관된 데이터 사용

![그림](/assets/img/jspservlet/0603/0603-19.png){: width="500" height="500"}
~~~
<%
Member member = (Member)session.getAttribute("member");
%>
~~~

### HttpSession 무효화

![그림](/assets/img/jspservlet/0603/0603-20.png){: width="500" height="500"}
~~~
HttpSession.session = request.getSession();
session.invalidate();
~~~

## 쿠키

웹브라우저에서 서버로 어떤 데이터를 요청하면, 서버측에서는 알맞은 로직을 수행한 후 데이터를 웹브라우저에 응답한다. 그리고, 서버는 웹브라우저와의 관계를 종료한다. 응답 후 관계를 끊는 것은 http프로토콜의 특징이다.
연결이 끊겼을 때 어떤 정보를 지속적으로 유지하기 위한 수단으로 쿠키라는 방식을 사용한다.
쿠키는 서버에서 생성하여, 서버가 아닌 클라이언트측에 특정 정보를 저장하고 서버에 요청 할 때 마다 쿠키의 속성값을 참조 또는 변경 할 수 있다.

~~~
<%
//쿠키생성
Cookie cookie = new Cookie("CookieName", "CookieValue");
cookie.setMaxAge(60*60); //쿠키 유효기간을 설정
response.addCookie(cookie); //response객체에 쿠키 탑재
%>
~~~

~~~
Cookie[] cookies = request.getCookies();
...
String str = cookies[i].getName();
...
~~~

### 정리

ServletContext는 웹 애플리케이션 시작 시 생성되어서 종료할때 가지 유지된다. 따라서 웹 애플리케이션이 실행되는 동안 공유할 객체나 데이터가 있다면 ServletContext에 보관한다.
HttpSession은 최초 로그인해서 로그아웃하거나 타임아웃 될 때 까지 유지되는 보관소다. 따라서 로그인하는 동안 페이지 데이터를 계속 유지할 필요가 있다면 HttpSession에 보관한다.
ServletRequest는 요청을 처리하는동안 유지되는 보관소이기 때문에 인클루딩하고 포워딩하는 서블릿들끼리 공유할 데이터가 있다면 ServletRequest에 보관한다.
JspContext는 Jsp페이지 마다 별도로 존재하는 보관소이기 때문에 Jsp페이지 내에서 태그 핸들러와 함께 공유할 데이터라면 JspContext에 보관한다.


🔍 **참고자료**

엄진영 유튜브 : <https://www.youtube.com/c/%EC%97%84%EC%A7%84%EC%98%81>

자바웹개발워크북 : <https://book.naver.com/bookdb/book_detail.nhn?bid=7623127>

유튜브 : <https://www.youtube.com/c/WizcenterSeoul>