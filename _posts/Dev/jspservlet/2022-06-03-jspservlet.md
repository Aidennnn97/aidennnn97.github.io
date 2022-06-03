---
layout: post
title: Dev-JSPServlet-post5
description: >
  JSP/Servlet
sitemap: false
categories:
  - dev
  - jspservlet
---

# MVC 아키텍처

기존의 웹 프로그래밍방식은 올인원 방식이라고 할 수 있다. 클라이언트의 요청처리를 서블릿 혼자서 모두 감당하기 때문이다. 
![그림](/assets/img/jspservlet/0603/0603-1.png){: width="500" height="500"}

MVC란 Model, View, Controller를 뜻하는 용어로 개발 형태이다. MVC 아키텍처의 특징은 역할을 세분화하고 의존성을 최소화하여 유지 보수가 쉽고, 중복 코드의 작성을 최소화하고 기존 코드의 재사용을 높힐 수 있다.

![그림](/assets/img/jspservlet/0603/0603-2.png){: width="500" height="500"}

- 컨트롤러(controller)컴포넌트의 역할
  - 클라이언트의 요청에 대해 모델과 뷰를 결정하여 전달하는 일종의 조정자 역할.
- 모델(model)컴포넌트의 역할
  - 데이터 저장소와 연동하여 사용자가 입력한 데이터나 사용자에게 출력할 데이터를 다루는 역할.
- 뷰(view)컴포넌트의 역할
  - 모델이 처리한 데이터나 그 작업 결과를 가지고 사용자에게 출력할 화면을 만드는 역할.

## 데이터 가져오기

![그림](/assets/img/jspservlet/0603/0603-3.png){: width="500" height="500"}

## MVC 기법

MVC 패턴을 적용하여 프로젝트를 만들 때 Model1 기법과 Model2 기법이 있다.

![그림](/assets/img/jspservlet/0603/0603-4.png){: width="500" height="500"}

**Model1**은 MVC에서 View와 Controller가 같이 있는 형태로, 프로젝트의 규모가 작고 유지보수보다 빨리 개발해야하는 것이 중요할 때 선호한다.

![그림](/assets/img/jspservlet/0603/0603-5.png){: width="500" height="500"}

**Model2**는 Model, View, Controller가 모두 모듈화 되어 있는 형태로, 프로젝트의 규모가 크고 유지보수가 수시로 계속일어날 때 사용하는 방식이다.

## 서블릿에서 뷰 분리하기

서블릿에서 하던 일 중에서 화면생성하는 일을 JSP가 맡게된다. 그렇게 되면 서블릿이 DBMS로 부터 가져온 데이터를 JSP에 던져주어야한다. 그러기 위해서는 또 값 객체(Value Object)라는 것을 만들게 된다. 값 객체는 데이터 수송 객체(Data Transfer Object)라고도 한다. 서블릿이 DBMS로 부터 데이터를 가져오면 가져온 데이터를 가지고 객체로 만들어 JSP에 던져주고 JSP는 받은 객체로 화면을 만들고 서블릿에 리턴해 준다.

![그림](/assets/img/jspservlet/0603/0603-10.png){: width="500" height="500"}

## 포워딩과 인클루딩

서블릿끼리 작업을 위임하는 방법에는 포워딩과 인클루딩 이렇게 두 가지 방법이 있다.
포워딩 방식은 작업을 한 번 위임하면 다시 이전 서블릿으로 제어권이 돌아오지 않는다.
인클루딩 방식은 다른 서블릿으로 작업을 위임한 후, 그 서블릿의 실행이 끝나면 다시 이전 서블릿으로 제어권이 넘어온다.

![그림](/assets/img/jspservlet/0603/0603-11.png){: width="500" height="500"}

- 포워딩
  -  웹브라우저가 '서블릿A'를 요청하면, 서블릿A는 작업을 수행한다.
  -  서블릿A에서 '서블릿B'로 실행을 위임한다.
  -  서블릿B는 작업을 수행하고 나서 응답을 완료하고, 서블릿A로 제어권이 돌아가지 않는다.

- 인클루딩
  -  웹브라우저가 '서블릿A'를 요청하면, 서블릿A는 작업을 수행한다.
  -  서블릿A에서 '서블릿B'로 실행을 위임한다.
  -  서블릿B는 작업을 수행하고 나서 다시 서블릿A로 제어권을 넘긴다.
  -  서블릿A는 나머지 작업을 수행한 후 응답을 완료한다.

🔍 **참고자료**

엄진영 유튜브 : <https://www.youtube.com/c/%EC%97%84%EC%A7%84%EC%98%81>

자바웹개발워크북 : <https://book.naver.com/bookdb/book_detail.nhn?bid=7623127>

유튜브 : <https://www.youtube.com/c/WizcenterSeoul>