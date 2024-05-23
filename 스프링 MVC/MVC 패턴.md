***
### MVC패턴의 필요성

우선 개발할 때 중요하다고 느낀 점은 객체지향으로 설계해야 한다는 것이다. </br>
하나의 코드에 파라미터, 비즈니스, 서비스, 뷰, 데이터 조회 등의 로직이 한 번에 있다면</br>
분명히 수정할 일이 생기는데 수정하기 굉장히 어려워지고 하나의 코드가 너무 많은 역할을 </br>
하게되면 서버가 다운되는 일도 있을 수 있을 것이다. </br>
어떤 변수가 있을지 모르니 객체지향으로 설계하는 것이 맞다 </br>
그렇게 나온게 MVC패턴이라고 이해했다.</br>

### Model View Controller
* 컨트롤러: HTTP 요청을 받아서 파라미터를 검증하고, 비즈니스 로직을 실행한다. </br>
            그리고 뷰에 전달할 결과 데이터를 조회해서 모델에 담는다
* 모델: 뷰에 출력할 데이터를 담아둔다. 뷰가 필요한 데이터를 모두 모델에 담아서 </br>
        전달해주는 덕분에 뷰는 비즈니스 로직이나 데이터 접근을 몰라도 되고, </br>
        화면을 렌더링 하는 일에 집중할 수 있다.
* 뷰: 모델에 담겨있는 데이터를 사용해서 화면을 그리는 일에 집중한다. 여기서는 HTML을 생성하는 부분을 말한다.


MVC패턴 - 적용
서블릿을 컨트롤러로, jsp를 뷰로 사용해 mvc 패턴을 적용해보자
model은 HttpServletRequest 객체를 사용, request는 내부에 데이터 저장소를 가지고 있다.
request.setAttribute(), request.getAttribute() 를 사용하면 데이터를 보관하고, 조회 가능

### 회원 등록

**회원 등록 폼 컨트롤러** </br>
MvcMemberFormServlet
```
@WebServlet(name = "mvcMemberFormServlet", urlPatterns = "/servlet-mvc/members/new-form")
public class MvcMemberFormServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String viewPath = "/WEB-INF/views/new-form.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
    }
}
```
`dispatcher.forward()`: 다른 서블릿이나 jsp로 이동할 수 있는 기능이다. **서버 내부**에서 다시 호출이 발생한다.

> ### redirect vs forward
> 리다이렉트는 실제 클라이언트(웹 브라우저)에 응답이 나갔다가, 클라이언트가 redirect경로로 다시 요청한다.
> 따라서 클라이언트가 인지할 수 있고, URL 경로도 실제로 변경된다. 반면에 포워드는 서버 내부에서 일어나는
> 호출이기 때문에 클라이언트가 전혀 인지하지 못한다.


### WEB-INF/jsp/members/new-form.jsp </br>
`/WEB-INF/`</br>
이 경로안에 jsp가 있으면 외부에서 직접 jsp를 호출할 수 없다.(클라이언트의 url요청) </br>
우리가 기대하는 것은 항상 컨트롤러를 통해서 jsp를 호출하는 것이다.
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!-- 상대경로 사용, [현재 URL이 속한 계층 경로 + /save] -->
<form action="save" method="post">
    username: <input type="text" name="username" />
    age:      <input type="text" name="age" />
    <button type="submit">전송</button>
</form>

</body>
</html>
```

***
### 회원 저장

**회원 저장 폼 컨트롤러** </br>
MvcMemberSaveServlet
```
@WebServlet(name = "mvcMemberSaveServlet", urlPatterns = "/servlet-mvc/members/save")
public class MvcMemberSaveServlet extends HttpServlet {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(age, username);
        memberRepository.save(member);

        //Model에 데이터를 보관
        request.setAttribute("member", member);

        String viewPath = "/WEB-INF/views/save-result.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
    }
}
```

### WEB-INF/jsp/members/save-result.jsp </br>
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
성공
<ul>
    <li>id=${member.id}</li>
    <li>username=${member.username}</li>
    <li>age=${member.age}</li>
</ul>
<a href="/index.html">메인</a>
</body>
</html>
```


***
### 회원 조회

**회원 조회 폼 컨트롤러** </br>
MvcMemberListServlet
```
@WebServlet(name = "mvcMemberListServlet", urlPatterns = "/servlet-mvc/members")
public class MvcMemberListServlet extends HttpServlet {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        List<Member> members = memberRepository.findAll();

        request.setAttribute("members", members);

        String viewPath = "/WEB-INF/views/members.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
    }
}
```

느낀점:</br>
뷰단과 자바로직이 거의 분리된 점이 확실히 깔끔했다. 하나의 코드에서
하나의 역할만 하는 것이 각 코드별로 부담이 되지 않을 듯 하였다 
위 두가지의 이유에서 유지보수하기 훨씬 편리할 것 같다.

하지만 강사님이 말씀하신대로

중복되는 로직이 많았다. 이건 어떻게 보완하면 좋을지,,
