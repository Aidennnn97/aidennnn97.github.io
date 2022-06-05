---
layout: post
title: Dev-JSPServlet-post9
description: >
  JSP/Servlet
sitemap: false
categories:
  - dev
  - jspservlet
---

# DBCP

이전에는 DB커넥션 객체를 하나만 만들어 사용했다. 하지만 이때는 아래의 그림과 같은 문제가 발생한다.

![그림](/assets/img/jspservlet/0605/0605-5.png){: width="500" height="500"}

즉, 싱글 커넥션을 공유하게 되면 같은 커넥션을 이용하는 DAO들의 작업에도 영향을 끼치게 된다.
해결책으로 각각의 DAO 별로 커넥션을 할당하려 하면 DAO가 작업을 하지 않을 때도 커넥션이 계속 서버에 연결된 채로 있기 때문에 자원이 낭비되는 문제가 있다.
한정된 자원을 효율적으로 사용하는 방법으로 **대여**라는 기법을 사용한다. 대여라는 방식을 프로그래밍세계에서는 풀링(pooling)기법이라 부르고 풀링기법을 DB커넥션에 적용한게 바로 **DB 커넥션풀**이다.
**커넥션풀**을 이용하면, 각 요청에 대해 별도의 커넥션 객체를 사용하기 때문에 다른 작업에 영향을 주지 않는다. 또한, 사용한 커넥션 객체는 버리지 않고 풀에 보관해 두었다가 다시 사용하기 때문에, 가비지가 생성되지 않고 속도도 빨라진다.

## DBCP 적용

![그림](/assets/img/jspservlet/0605/0605-6.png){: width="500" height="500"}

>1. ProjectDao에서 커넥션 요청 : getConnection()
>
>2. DBCP에서 Connection(100) 생성
>
>3. 만든 커넥션을 ProjectDao로 리턴

![그림](/assets/img/jspservlet/0605/0605-7.png){: width="500" height="500"}

>1. MemberDao에서 커넥션 요청 : getConnection()
>
>2. DBCP에서 Connection(101) 생성
>
>3. 만든 커넥션을 MemberDao로 리턴

![그림](/assets/img/jspservlet/0605/0605-8.png){: width="500" height="500"}

>1. ProjectDao에서 커넥션 반납 : returnConnection()
>
>2. DBCP에서 Connection(100) 보관

![그림](/assets/img/jspservlet/0605/0605-9.png){: width="500" height="500"}

>1. TaskDao에서 커넥션 요청 : getConnection()
>
>2. DBCP에서 보관중인 Connection(100)을 TaskDao로 리턴

## DataSource와 JNDI

### DataSource

DataSource는 DriverManager를 통해 DB커넥션을 얻는 것보다 더 좋은 기법을 제공한다. 특히 다음 두 가지 점에서 DriverManager 보다 낫다.

첫째, DataSource는 서버에서 관리하기 때문에 데이터베이스나 JDBC 드라이버가 변경되더라도 애플리케이션을 바꿀 필요가 없다.
DriverManager를 사용하는 경우, 웹 애플리케이션에서 관리하기 때문에 데이터베이스의 주소가 바뀐다거나 JDBC드라이버가 변경될 경우 웹 애플리케이션의 코드도 변경해야 한다.

둘째, DataSource를 사용하면 Connection과 Statement객체를 풀링할 수 있으며, 분산 트랜잭션을 다룰 수 있다. DataSource는 자체적으로 커넥션풀 기능을 구현하기 때문에 웹 애플리케이션 쪽에서 따로 작업할 것이 없어 매우 편리하다.

### 톰캣 서버에 DataSource 설정하기

tomcat컨테이너가 데이터베이스 인증을 하도록 context.xml파일을 열어 아래의 코드를 추가 한다.

~~~
<Resource
    auth = ""
    driverClassName = ""
    url = ""
    username = ""
    password = ""
    name = ""
    type = ""
    maxActive = ""
    maxWait = ""
 />
~~~

### JNDI

JNDI는 Java Naming and Directory Interface API의 머리글자이다. 디렉터리 서비스에 접근하는데 필요한 API이며 애플리케이션은 이 API를 사용하여 서버의 자원을 찾을 수 있다. 자원이라함은 데이터베이스 서버나 메시징 시스템과 같이 다른 시스템과의 연결을 제공하는 객체이다. 특히 JDBC 자원을 데이터 소스라고 부른다.

자원을 서버에 등록할 때는 고유한 JNDI 이름을 붙인다. JNDI 이름은 사용자에게 친숙한 디렉터리 경로 행태를 가진다. 예를 들어 JDBC 자원에 대한 JNDI 이름은 jdbc/mydb의 형식으로 짓는다. 다음은 Java EE 애플리케이션 서버에서 자원을 찾을 때 기본 JNDI 이름이다.

| java:comp/env            | 응용 프로그램 환경 항목   |
| java:comp/env/jdbc       | JDBC 데이터 소스       |
| java:comp/ejb            | EJB 컴포넌트          |
| java:comp/UserTransaction| UserTransaction 객체 |
| java:comp/env/mail       | JavaMail 연결 객체    |
| java:comp/env/url        | URL 정보             |
| java:comp/env/jms        | JMS 연결 객체         |


🔍 **참고자료**

엄진영 유튜브 : <https://www.youtube.com/c/%EC%97%84%EC%A7%84%EC%98%81>

자바웹개발워크북 : <https://book.naver.com/bookdb/book_detail.nhn?bid=7623127>

유튜브 : <https://www.youtube.com/c/WizcenterSeoul>