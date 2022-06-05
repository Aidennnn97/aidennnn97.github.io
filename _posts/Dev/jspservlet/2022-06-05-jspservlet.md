---
layout: post
title: Dev-JSPServlet-post8
description: >
  JSP/Servlet
sitemap: false
categories:
  - dev
  - jspservlet
---

# DAO 만들기

서블릿으로부터 분리해야 하는 기능은 데이터베이스와 연동하여 데이터를 처리하는 부분이다. 이렇게 데이터 처리를 전문으로 하는 객체를 **DAO(data access object)**라고 부른다.
DAO는 데이터베이스나 파일, 메모리 등을 이용하여 애플리케이션 데이터를 생성, 조회, 변경, 삭제하는 역할을 수행한다. 업무 로직에서 데이터 처리 부분을 분리하여 별도의 객체로 정의하면, 여러 업무에서 공통으로 사용할 수 있기 때문에 유지보수가 쉬워지고 재사용성이 높아진다.

![그림](/assets/img/jspservlet/0605/0605-1.png){: width="500" height="500"}

## 클라이언트 요청처리 흐름도

![그림](/assets/img/jspservlet/0605/0605-2.png){: width="500" height="500"}

## ServletContextListener와 객체 공유

서블릿 컨테이너는 웹 애플리케이션의 상태를 모니터링 할 수 있도록 웹 애플리케이션의 시작에서 종료까지 주요한 사건에 대해 알림 기능을 제공한다. 알림 기능을 이용하고 싶다면, 규칙에 따라 객체를 만들어 DD파일(web.xml)에 등록하면 된다. 이렇게 사건이 발생했을 때 알림을 받는 객체를 '리스너'라고 부른다.

![그림](/assets/img/jspservlet/0605/0605-3.png){: width="500" height="500"}

호출되는 메서드의 규칙을 정의한게 바로 ServletContextListener라는 인터페이스이다.

![그림](/assets/img/jspservlet/0605/0605-4.png){: width="500" height="500"}

ContextLoaderListener를 사용하려면 서블릿 컨테이너에 등록해야한다. 즉, 배치를 해야한다.

~~~ web.xml
<listener>
    <listener-class>spms.listeners.AppInitServlet</listener-class>
</listener>
~~~

🔍 **참고자료**

엄진영 유튜브 : <https://www.youtube.com/c/%EC%97%84%EC%A7%84%EC%98%81>

자바웹개발워크북 : <https://book.naver.com/bookdb/book_detail.nhn?bid=7623127>

유튜브 : <https://www.youtube.com/c/WizcenterSeoul>