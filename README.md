# newlec_jsp

## 2021.06.08 뉴렉처 JSP 웹 프로그래밍 파일생성

- 파일 출처: http://www.newlecture.com

## 2021.06.09
### 51강 내용

- 강의에 쓰이는 자료 다운로드 받기
  - 파일 출처: http://www.newlecture.com

- notice directory list.html -> list.jsp로 복사
  - 한글이 깨지면 **File -> Properties -> UTF-8로 변경**
  - **주의** html에서 jsp로 변경후 아무것도 건들지 않아야 적용됨
- 공지사항을 출력해주는 **<tr>** 태그라인 1개 빼고 삭제
<hr><br>
  
### JDBC를 이용해 글 목록 구현하기 - 52강 내용

- Database를 연동하기 위해 import 추가
  - list.jsp 맨위 상단에 적용
  ``` java
  String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
  String sql = "SELECT * FROM NOTICE";

  Class.forName("oracle.jdbc.driver.OracleDriver");
  Connection con = DriverManager.getConnection(url, "newlec", "wjswlsdhks12");
  Statement st = con.createStatement();
  ResultSet rs = st.executeQuery(sql);
  ```
  
  - list.jsp </html> 맨 하단에 적용
  ``` java
  rs.close();
  st.close();
  con.close();
  ```
  <br>
  
- **추가적으로 해야할 것**
  - ~~Oracle Database(DBMS)설치~~
  - ~~OJDBC Driver 설치~~
  - ~~Mac OS 환경은 Doker를 이용해야 오라클 설치가 가능하다고 함 (확인해봐야 할것)~~

- 추가 내용
  - Java와 DB를 연결하기 위해 JDBC를 사용해야한다.
  - Mac OS 기준 Docker에 Oracle DB(이미지)설치하고 사용해야 할 테이블을 생성한다.
  - Eclipse의 해당 프로젝트에 "**ojdbc.jar**" 파일을 WEB-INF/lib/ 경로에 저장한다.
    - ojdbc의 버전은 "**JRE1.8**", "**Oracle11g**" Version 기준으로 ojdbc6.jar을 사용한다.

Java JDBC코드에 오타가 없는지 잘 살펴본다.<br>
2~3일동안 구글링으로 계속 찾아봐도 오류해결이 안되서 코드를 살펴봤더니 oracle에서 "a"가 빠져있었다..
<hr><br>

### 자세한 페이지 구현하기 - 53강 내용

- 변경사항
  - list.jsp
- 추가사항
  - detail.html -> detail.jsp
  
#### list.jsp

- 게시글의 번호(ID) 가져오기
  - 정적이였던 게시글의 번호(ID)를 정적으로 변경해 준다.
  - 변경전: ```<a href="detail.html"><%=rs.getString("TITLE") %>```
  - 변경후: ```<a href="detail.jsp?id=<%=rs.getInt("ID")%>">```

#### detail.jsp

- 사용자가 게시글 클릭후 클릭한 게시글로 이동한다.
  - list.jsp가 게시글의 목록을 나타냈다면, detail.jsp는 게시글의 내부를 나타낸다.
  - JDBC를 연동하기 위해 import와 추가 코드를 작성한다.
  ``` java
  <%@page import="java.sql.ResultSet"%>
  <%@page import="java.sql.PreparedStatement"%>
  <%@page import="java.sql.DriverManager"%>
  <%@page import="java.sql.Connection"%>
  <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

  <%
  int id = Integer.parseInt(request.getParameter("id"));
	String url = "jdbc:oracle:thin:@localhost:1521:xe";
	String sql = "SELECT * FROM NOTICE WHERE ID=?";
	Class.forName("oracle.jdbc.driver.OracleDriver");
	Connection con = DriverManager.getConnection(url, "newlec", "wjswlsdhks12");
	PreparedStatement st = con.prepareStatement(sql);
	st.setInt(1, id);
	
	ResultSet rs = st.executeQuery();
	rs.next();
  %>
  ```
  
- list.jsp와 detail.jsp의 JDBC코드 차이점
  - list.jsp
  ``` java
  Statement st = con.createStatement();
  ResultSet rs = st.executeQuery(sql);
  ```
  <br>
  
  - detail.jsp
  ``` java
  int id = Integer.parseInt(request.getParameter("id"));
  PreparedStatement st = con.prepareStatement(sql);
  st.setInt(1, id);
	
  ResultSet rs = st.executeQuery();
  rs.next();
  ```
<hr><br>
detail.jsp의 코드에 대한 추가적인 설명은 정리 후 작성 예정
