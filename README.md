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
<br>
  
### JDBC를 이용해 글 목록 구현하기 - 52강 내용

- Database를 연동하기 위해 import 추가
  - list.jsp 맨위 상단에 적용
  ``` java
  String url = "jdbc:orcle:thin:@localhost:1521/xepdb1";
  String sql = "SELECT * FROM NOTICE";

  Class.forName("oracle.jdbc.driver.OracleDriver");
  Connection con = DriverManager.getConnection(url, "newlec", "skdine");
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
  - Oracle Database(DBMS)설치
  - OJDBC Driver 설치
  - Mac OS 환경은 Doker를 이용해야 오라클 설치가 가능하다고 함 (확인해봐야 할것)
