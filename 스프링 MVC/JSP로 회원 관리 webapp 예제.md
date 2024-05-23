***
## JSP로 회원 관리 웹 애플리케이션 만들기

### JSP 라이브러리 추가
JSP를 사용하려면 라이브러리 추가해줘야 한다.
**build.gradle**
```
	//JSP 추가 시작
	implementation 'org.apache.tomcat.embed:tomcat-embed-jasper'
	implementation 'javax.servlet:jstl'
        implementation 'jakarta.servlet:jakarta.servlet-api' //스프링부트 3.0 이상
	implementation 'jakarta.servlet.jsp.jstl:jakarta.servlet.jsp.jstl-api' //스프링부트 3.0 이상
	implementation 'org.glassfish.web:jakarta.servlet.jsp.jstl' //스프링부트 3.0 이상
	//JSP 추가 끝
```

* ### 회원등록폼
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="/jsp/members/save.jsp" method="post">
    username: <input type="text" name="username" />
    age:      <input type="text" name="age" />
    <button type="submit">전송</button>
</form>
</body>
</html>
```

![image](https://github.com/Leejieok/TIL/assets/165024639/d2523806-6cc3-44e5-a84a-5ad9683c802b)


* ### save.jsp
```
<%@ page import="hello.servlet.domain.member.Member" %>
<%@ page import="hello.servlet.domain.member.MemberRepository" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    //request, response 사용 가능
    MemberRepository memberRepository = MemberRepository.getInstance();

    System.out.println("MemberSaveServlet.service");
    String username = request.getParameter("username");
    int age = Integer.parseInt(request.getParameter("age"));

    Member member = new Member(age, username);
    memberRepository.save(member);
%>
<html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
성공
<ul>
    <li>id=<%=member.getId()%></li>
    <li>id=<%=member.getUsername()%></li>
    <li>id=<%=member.getAge()%></li>
</ul>
<a href="/index.html">메인</a>
</body>
</html>
```

![image](https://github.com/Leejieok/TIL/assets/165024639/211de56a-2498-42a2-9761-3b7e4623d41a)

* ### save.jsp
회원목록
```
<%@ page import="hello.servlet.domain.member.Member" %>
<%@ page import="java.util.List" %>
<%@ page import="hello.servlet.domain.member.MemberRepository" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    MemberRepository memberRepository = MemberRepository.getInstance();
    List<Member> members = memberRepository.findAll();
%>
<html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<a href="/index.html">메인</a>
<table>
    <thead>
    <th>id</th>
    <th>username</th>
    <th>age</th>
    </thead>
    <tbody>
    <%
        for(Member member : members) {
            out.write("    <tr>");
            out.write("        <td>" + member.getId() + "</td>");
            out.write("        <td>" + member.getUsername() + "</td>");
            out.write("        <td>" + member.getAge() + "</td>");
            out.write("    </tr>");
        }
    %>
    </tbody>
</table>
</body>
</html>
```
![image](https://github.com/Leejieok/TIL/assets/165024639/883baa9e-88a4-44be-92aa-0252a7d0e8b2)


***
[예전에 JSP로 만들었던게 있었다.](https://github.com/Leejieok/MVC_jsp.git) </br>

만들면서 불편하다고 느낀 점</br>
간단한 회원가입폼을 만드는데에도 자바언어를 사용해 비즈니스 로직이 jsp파일에도 많이 필요했다. </br>
또 뒷단 쪽에서 가져온 데이터를 html과 함께 쓸 수 있는건 장점인 동시에 단점도 되었다. </br>
로직이 지저분해지고 다른사람이 읽었을 때 이해하기 힘들게 만들어졌다. </br>
간단한 작업도 지저분하게 되는데 현업 로직을 상상했을 땐 살짝 섬뜩했다 </br>
보다 보기 쉽고 깔끔한 로직을 구현하는 개발자가 되고싶다 </br>

![image](https://github.com/Leejieok/TIL/assets/165024639/bad73062-ecac-4cb5-93f5-76d3663caf6c)</br>
![image](https://github.com/Leejieok/TIL/assets/165024639/dba1054e-a055-454b-8b98-f5a5a6a14b58)</br>
![image](https://github.com/Leejieok/TIL/assets/165024639/25c4b5bd-cdd5-443b-8d16-21e989288c15)</br>
![image](https://github.com/Leejieok/TIL/assets/165024639/a2ca8558-834a-40e4-ac64-175ba56c41ff)</br>
![image](https://github.com/Leejieok/TIL/assets/165024639/c6922d31-5447-4c03-bb45-8021e25c1351)</br>





