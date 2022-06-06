---
layout: post
title: Dev-JSPServlet-post10
description: >
  JSP/Servlet
sitemap: false
categories:
  - dev
  - jspservlet
---
# 미니 MVC 프레임워크 만들기

## 프런트 컨트롤러의 도입

![그림](/assets/img/jspservlet/0606/0606-1.png){: width="500" height="500"}

프런트 컨트롤러 도입 전에는 웹 브라우저 요청이 들어오면 컨트롤러 역할을 수행하는 서블릿에서 직접 요청을 받아 모델과 뷰를 통해 작업을 수행했다. 컨트롤러들이 하는 일 중에서 공통되는 일이 있다. JSP로 실행을 위임하거나 다른 페이지로 리다이렉트 시키거나 파라미터를 값 객체로 만드는 일 등.

![그림](/assets/img/jspservlet/0606/0606-2.png){: width="500" height="500"}

 컨트롤러가 하는 일 중에서 공통 또는 반복적으로 하는 작업들을 추출하여 별도의 객체로 분리시켜 만든 것을 프런트 컨트롤러라 부른다.
 프런트 컨트롤러를 도입하면 페이지 컨트롤러의 코드가 간결해지고, 페이지 컨트롤러는 더이상 서블릿일 필요가 없어진다.

## 프런트 컨트롤러 패턴

웹 브라우저에서 요청이 들어오면 프런트 컨트롤러가 받을 수 있도록 DispatcherServlet이라는 클래스를 만들고 클라이언트 요청을 받아야 하기 때문에 서블릿이어야 한다.

![그림](/assets/img/jspservlet/0606/0606-3.png){: width="500" height="500"}

DispatcherServlet은 페이지 컨트롤러를 실행하기 전에 공통 작업을 처리해야 하기 때문에, 요청 URL에 대한 규칙을 정의할 필요가 있다. 즉 .do 접미사가 붙은 요청이 들어오면 그 요청은 DispatcherServlet에서 처리하게 한다.

![그림](/assets/img/jspservlet/0606/0606-4.png){: width="500" height="500"}

.do 로 끝나는 URL요청을 처리하게 하려면 서블릿 선언에 URL 매핑 정보를 설정해야 한다.

## 프런트 컨트롤러 요청 처리 과정

![그림](/assets/img/jspservlet/0606/0606-5.png){: width="500" height="500"}

웹 브라우저로부터 *.do 요청이 들어오면 DispatcherServlet이 받고 DispatcherServlet은 클라이언트가 요청한 서블릿이 어떤 서블릿인지 ServletPath를 알아낸다.

![그림](/assets/img/jspservlet/0606/0606-6.png){: width="500" height="500"}

ServletPath에 해당하는 페이지 컨트롤러를 찾아 실행을 위임한다.

![그림](/assets/img/jspservlet/0606/0606-7.png){: width="500" height="500"}

페이지 컨트롤러는 작업을 마친 후, 어느 JSP로 가야할지 DispatcherServlet에 JSP URL 또는 리다이렉트 URL을 리턴한다.

![그림](/assets/img/jspservlet/0606/0606-8.png){: width="500" height="500"}

프런트 컨트롤러는 페이지 컨트롤러가 알려준 URL정보를 가지고 JSP로 실행을 위임한다.

![그림](/assets/img/jspservlet/0606/0606-9.png){: width="500" height="500"}

리다이렉트 URL로 리턴 받았다면 리다이렉트로 응답을 처리한다.

## 페이지 컨트롤러의 진화

클라이언트 요청은 프런트 컨트롤러가 받기 때문에 페이지 컨트롤러는 서블릿이 될 필요가 없다.

![그림](/assets/img/jspservlet/0606/0606-10.png){: width="500" height="500"}

페이지 컨트롤러가 서블릿이 아니면 프런트 컨트롤러에서 페이지 컨트롤러로 실행을 위임할 때 RequestDispatcher 객체를 사용해서 including을 할 수 없다. 따라서 페이지 컨트롤러로 실행을 위임할 새로운 호출 규칙이 필요하다.

![그림](/assets/img/jspservlet/0606/0606-11.png){: width="500" height="500"}

프런트 컨트롤러와 페이지 컨트롤러 사이에 controller라는 interface를 정의했다. 인터페이스는 호출규칙을 정의하는데 사용하는 문법이다. 즉, 호출자와 피호출자 사이의 규칙을 정의할 때는 interface 문법을 사용해서 정의하는 것이다.
execute가 바로 프런트 컨트롤러인 DispatcherServlet이 페이지 컨트롤러에 대해서 호출하는 메서드가 된다.
execute의 파라미터로 Map 객체가 넘어오는데 이 Map 객체가 프런트 컨트롤러와 페이지 컨트롤러 사이에 데이터 교환을 위한 일종의 바구니이다. execute의 리턴 값은 페이지 컨트롤러가 작업을 끝낸 후, 어떤 JSP로 가야하는지, 어디로 리다이렉트 해야하는지 JSP경로 또는 리다이렉트 URL을 리턴한다.


🔍 **참고자료**

엄진영 유튜브 : <https://www.youtube.com/c/%EC%97%84%EC%A7%84%EC%98%81>

자바웹개발워크북 : <https://book.naver.com/bookdb/book_detail.nhn?bid=7623127>

유튜브 : <https://www.youtube.com/c/WizcenterSeoul>