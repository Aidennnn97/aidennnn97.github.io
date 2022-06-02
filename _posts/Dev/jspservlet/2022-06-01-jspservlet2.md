---
layout: post
title: Dev-JSPServlet-post4
description: >
  JSP/Servlet
sitemap: false
categories:
  - dev
  - jspservlet
---

# 서블릿과 JDBC

DBMS가 등장하기 전에는 애플리케이션 개발자들은 직접 파일 입출력 스트리밍 API를 사용하여 하드디스크에 데이터를 저장하고 꺼내는 작업을 하였다. 그러다가 DBMS가 등장하면서 개발자를 대신하여 파일 입출력 API를 사용하여 작업을 하고, 개발자는 DBMS를 사용하게 되었다. 그런데 개발자가 DBMS를 사용할때 상호간의 표준화된 사용규칙이 존재했고 이를위해 등장한것이 **SQL**이다.

- SQL(Structured Query Language)?
> 애플리케이션이 DBMS를 사용할 때 일관성있게 쓸 수 있도록 사용규칙을 표준화한 일종의 구조화된 질의어이다.

![그림](/assets/img/jspservlet/0601/0601-7.png){: width="500" height="500"}

위의 그림의 애플리케이션과 DBMS간의 통신 프로토콜이 DBMS마다 전용 프로토콜을 사용해야하는 문제가있다.
즉, 개발자는 각각의 DBMS 전용 프로토콜에 따라서 통신할 수 있도록 소켓 프로그래밍을 작성해야 한다는 것이다.
그래서 DBMS를 만든 회사에서는 DBMS에 접속할 수 있도록 Vendor(Native)API를 제공한다.
하지만 다른 DBMS끼리는 통신할 수 없었고 DBMS에 종속되는 문제가 발생하게 되었다.
여러가지 DBMS를 사용하면 개발비가 증가하게 되었고 이러한 문제를 해결하기위해 DBMS에 접근하기 위한 표준 인터페이스인 ODBC(Open DataBase Connectivity)가 등장하게 되었다.

![그림](/assets/img/jspservlet/0601/0601-8.png){: width="500" height="500"}

그러나 ODBC가 다이렉트로 DBMS에 연결되는 것이 아니라 ODBC API에서 Vendor API로 호출하는 식으로 되어있어서 호출 단계가 추가 되어 속도가 느려지는 문제가 발생했고, Java애플리케이션에서 DBMS에 접속해야 하는데 ODBC는 C/C++로 되어있어서 Java에서 호출할 수 없었다. 그래서 Java 애플리케이션을 위한 DBMS 접속 인터페이스인 JDBC를 정의하게 되었다.

![그림](/assets/img/jspservlet/0601/0601-9.png){: width="500" height="500"}

## 데이터베이스에서 데이터가져오기

### 1. 사용할 JDBC 드라이버 등록

> DriverManager.registerDriver(new oracle.jdbc.driver.OracleDriver());
> 
> Class.forName("oracle.jdbc.OracleDriver");

Class.forName()과 DrivaerManager.registerDriver()의 차이점?
Class.forName()는 DrivaerManager.registerDriver()를 호출함으로써 JDBC드라이버를 등록하고, 인스턴스를 생성함.
일반적으로는 Class.forName()을 통해 드라이버를 등록하며, 실제 JDBC드라이버를 만드는 경우가 아니라면 DriverManager클래스를 제어하고 사용할 일이 없기 때문에 DriverManager.registerDriver()메서드는 드라이버를 구현하는 내부에서 사용됨.

### 2. 드라이버를 사용하여 DBMS서버와 연결요청

> Connection con = DriverManager.getConnection("jdbcUrl", "userid", "userpwd");


### 3. 커넥션 객체로부터 SQL을 던질 객체 준비

> Statement stmt = con.createStatement();

createStatement()가 반환하는것은 java.sql.Statement 인터페이스의 구현체이다. 이 객체를 통해 데이터베이스에 SQL문을 보낼 수 있다. Statement 인터페이스에는 데이터베이스에 질의하는데 필요한 메서드가 정의되어 있고, 이 인터페이스를 구현했다는 것은 반환 객체가 이러한 메서드들을 가지고 있다는 뜻이다.

### 4. SQL을 던지는 객체를 사용하여 서버에 질의

>  ResultSet rs = stmt.executeQuery("select ENO, ENAME, EMAIL, EDATE" + " from EMPLOYEE" + " order by ENO DESC");

질의를 하게되면 executeQuery가 서버에서 데이터를 가져올 ResultSet이라는 객체를 리턴한다

### 5. 서버에서 가져온 데이터를 사용하여 HTML을 만들어 웹 브라우저로 출력

~~~
Connection con = null;
Statement stmt = null;
ResultSet rs = null;

response.setContentType("text/html; charset="UTF-8");
PrintWriter out = response.getWriter();

while(rs.next()){
  out.println(rs.getInt("ENO") + ", " + rs.getString("EMAIL") + ", " + ...)
}

finally{
  rs.close();
  stmt.close();
  con.close(); // 자원해제할 때는 거꾸로
}
~~~

ResultSet 객체를 통해 next()를 호출하면 서버에서 레코드(행)를 가져온다. 서버에서 받은 레코드는 ResultSet 객체에 보관되고 next()는 서버에서 레코드를 받으면 true, 레코드가 없다면 false를 반환한다.
JDBC프로그래밍을 할 때 주의할 점은 정상적으로 실행되든 오류가 발생하든 간에 반드시 자원해제를 수행하는 것이다.
자원을 해제하기 가장 좋은 위치는 finally 블록이다. finally 블록은 try나 catch를 벗어나기 전에 반드시 수행된다.

## HttpServlet으로 GET, POST 요청 다루기

![그림](/assets/img/jspservlet/0601/0601-10.png){: width="500" height="500"}

~~~
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  ...
}
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  ...
}
~~~

클라이언트 요청이 들어오면, 첫째로 상속받은 HttpServlet의 service()메서드가 호출되고, service()는 클라이언트 요청 방식에 따라 doGet()이나 doPost() 등의 메서드를 호출한다.

## PreparedStatement의 사용

~~~
PreparedStatement pstmt = con.prepareStatement("insert into EMPLOYEES(EMAIL, ID, ENO, DATE)" + " values(?, ?, ?, NOW())";);
pstmt.setString(1, request.getParameter("email"));
pstmt.setInt(3, request.getParameter("eno"));
~~~

입력매개변수는 SQL문에서 "?" 문자로 표시된 입력항목을 가리키는 말이다. 입력매개변수에 할당할 데이터의 타입에 따라 적절한 메서드를 선택해야 한다.


## 리프레시

리프레시는 하나의 화면에서 다른 화면으로 자동으로 이동하게할 때 사용하는 기술이다. 

### 응답헤더를 이용한 리프레시
~~~
//리프레시 정보를 응답 헤더에 추가
response.setHeader("Refresh", "1;url=list"); // 1초후 목록페이지로 이동

response.addHeader("Refresh", "1;url=list"); // 위와 동일.
~~~

GET 요청 --> **https://localhost:9999/web04/member/list**

![그림](/assets/img/jspservlet/0601/0601-11.png){: width="500" height="500"}

작업결과를 출력한 후 다른 페이지로 이동할 때는 리프레시를 사용한다.

### HTML의 meta태그를 이용한 리프레시

~~~
<head>
<title>회원등록결과</title>
<meta http-equiv='Refresh' content='1; url=list'>
</head>
~~~
HTML의 \<head> 태그 안에 리프레시를 설정하는 \<meta> 태그를 추가한다.

## 리다이렉트

정보를 등록하고 나서 그 결과를 출력하지 않고 즉시 목록 화면으로 이동하게 하는 방법을 리다이렉트 라고 한다.

GET 요청 --> **https://localhost:9999/web04/member/list**

![그림](/assets/img/jspservlet/0601/0601-12.png){: width="500" height="500"}

~~~
response.sendRedirect("list");
~~~

## 서블릿 초기화 매개변수

서블릿 초기화 매개변수는 서블릿을 생성하고 초기화할 때, 즉 init()을 호출할 때 서블릿 컨테이너가 전달하는 데이터이다. 서블릿 초기화 매개변수는 DD(Deployment Descriptor)파일(web.xml)의 서블릿 배치 정보에 설정할 수 있고, 어노테이션을 사용하여 서블릿 소스 코드에 설정할 수 있다. 가능한 소스 코드에서 분리하여 외부 파일에 두는 것을 추천하는데 이는 외부 파일에 두면 변경하기 쉽기 때문이다.

![그림](/assets/img/jspservlet/0601/0601-13.png){: width="500" height="500"}

### web.xml에 설정방법

~~~
<servlet>
...
  <init-param>
      <param-name>매개변수 이름</param-name>
      <param-value>매개변수 값</param-value>
  </init-param>
...
</servlet>
~~~

### 어노테이션으로 설정

~~~
@WebServlet(
  urlPatterns={"/member/update"},
  initParams={
    @WebInitParam(name="driver", value="com.oracle.jdbc.Driver"),
    @WebInitParam(name="username", value="study"),
    ...
  }
)
public class MemberListServlet extends HttpServlet { ... }
~~~

## 컨텍스트 초기화 매개변수

서블릿 초기화 매개변수는 다른 서블릿은 참조할 수 없고 말 그대로 그 매개변수가 선언된 해당 서블릿에서만 사용가능하지만 컨텍스트 초기화 매개변수는 모든 서블릿이 사용 가능하다.

![그림](/assets/img/jspservlet/0601/0601-14.png){: width="500" height="500"}

### web.xml에 설정방법

~~~
<web-app>
...
  <context-param>
      <param-name>매개변수 이름</param-name>
      <param-value>매개변수 값</param-value>
  </context-param>
...
</web-app>
~~~

## 필터

필터는 서블릿 실행 전후에 어떤 작업을 하고자 할 때 사용하는 기술이다. 예를 들어 클라이언트가 보낸 데이터의 암호를 해제한다거나, 서블릿이 실행되기 전에 필요한 자원을 미리 준비한다거나, 서블릿이 실행될 때마다 로그를 남긴다거나 하는 작업을 필터를 통해 처리할 수 있다.(로그 출력, 사용자 인증 및 권한 검사, 암호화 및 복호화, 데이터 압축 및 해제, 이미지 변환 등)
만약 이런 작업들을 서블릿에 담는다면 서블릿마다 해당코드를 삽입해야 하고, 필요없어지면 찾아서 제거해야 하므로 관리가 매우 번거로울 것이다. 하지만 필터는 별도의 클래스로 분리되어 있기 때문에 언제든지 필요하면 사용하고 필요없으면 제거해주면 된다.

![그림](/assets/img/jspservlet/0601/0601-15.png){: width="500" height="500"}

![그림](/assets/img/jspservlet/0601/0601-16.png){: width="500" height="500"}

### 필터 적용 (web.xml)
~~~
<servlet>
  ...
  <filter>
      <filter-name>필터이름</filter-name>
      <filter-class>필터클래스</filter-class> //패키지 이름을 포함한 클래스이름
  </filter>

  <filter-mapping>
      <filter-name>필터이름</filter-name>
      <url-pattern>URL패턴</url-pattern> //필터를 언제 수행할지 지정, 해당 URL로 요청이 들어왔을때 만 필터실행
  </filter-mapping>
  ...
</servlet>
~~~

### 필터 적용(어노테이션)
~~~
@WebFilter(urlPatterns="/*") // /*  --> 어떤 요청이들어와도 모두실행

@WebFilter(
  urlPatterns="/*",
  initParams={
    @WebInitParam(
      name="encoding", value="UTF-8")
  })
~~~

### 필터 사용하기
~~~
@Override
public void doFilter(ServletRequest request, ServletResponse response, FilterChain nextFilter) throws IOException, ServletException {
  
  // 서블릿을 실행하기 전에 수행할 작업
  request.setCharacterEncoding("UTF-8");

  nextFilter.doFilter(request, response);

  // 서블릿을 실행한 후 수행할 작업
}
~~~


🔍 **참고자료**

엄진영 유튜브 : <https://www.youtube.com/c/%EC%97%84%EC%A7%84%EC%98%81>

자바웹개발워크북 : <https://book.naver.com/bookdb/book_detail.nhn?bid=7623127>