---
layout: post
title: Dev-JSPServlet-post2
description: >
  JSP/Servlet
sitemap: false
categories:
  - dev
  - jspservlet
---

# 웹 프로그래밍 기초 다지기

## HTTP 프로토콜의 이해

HTTP 프로토콜은 웹 브라우저와 웹 서버 사이의 데이터 통신 규칙이다. 사용자가 웹 페이지의 링크를 클릭하면 웹 브라우저는 HTTP 요청 형식에 따라 웹 서버에 데이터를 보내고, 웹 서버는 웹 브라우저가 보낸 데이터를 분석하여 요청 받은 일을 처리하여 응답한다.

![그림](/assets/img/jspservlet/0529/0529-1.png){: width="500" height="500"}

## GET 요청
+ GET 요청1 - 웹 브라우저 주소창에 URL을 입력하는경우
+ GET 요청2 - 링크를 클릭하는 경우
+ GET 요청3 - 입력 폼의 method 속성값이 get인 경우

~~~
<form action = "" method = "get"></form>
~~~

GET으로 요청하는 경우 서버에 보낼 데이터는 URI에 붙인다.
> GET /web02/CalculatorServlet>v1=20&op=%2B&v2=30 HTTP/1.1

![그림](/assets/img/jspservlet/0529/0529-2.png){: width="500" height="500"}

### 특징
+ URL에 데이터를 포함 -> 데이터 조회에 적합
+ 바이너리 및 대용량 데이터 전송 불가
+ 요청라인과 헤드 필드의 최대 크기
+ 보안에 취약

## POST 요청

~~~
<form action = "" method = "post"></form>
~~~

![그림](/assets/img/jspservlet/0529/0529-3.png){: width="500" height="500"}

### 특징
+ URL에 데이터가 포함되지 않음 -> 외부 노출 방지
+ 메시지 본문에 데이터 포함 -> 실행 결과 공유 불가
+ 바이너리 및 대용량 데이터 전송 가능

## 정리
HTTP 프로토콜에서 가장 많이 사용되는 요청 형식은 **GET**과 **POST**이다. **GET**은 브라우저의 주소창에 직접 URL을 입력하거나 사용자가 링크를 클릭하는 경우, 검색처럼 입력 폼을 통해서도 요청을 수행할 수 있다.
그러나 사용자가 입력한 데이터가 주소창에 그대로 노출되기 때문에 로그인이나 결제정보와 같은 데이터는 GET방식이 아닌 POST방식으로 보내야한다. **POST**방식은 웹 서버에 데이터를 보낼 때 메시지 본문 부분에 붙여서 보내기 때문에 주소창에 노출될 위험이 없고 데이터의 크기에 제한이 없다.

🔍 **참고자료**

엄진영 유튜브: <https://www.youtube.com/c/%EC%97%84%EC%A7%84%EC%98%81>

자바웹개발워크북 : <https://book.naver.com/bookdb/book_detail.nhn?bid=7623127>