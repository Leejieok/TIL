---
  프로젝트 생성 war 파일로 </br>
  jsp를 사용하려면 war를 사용해야함 </br>
  보통 war는 톰캣 서버를 별도로 설치하고 </br>
  war를 따로 빌드할 때 사용함. </br>
  하지만 톰캣 내장도 가능함 </br>
  dependenciesL: Spring Web, lombok

![image](https://github.com/Leejieok/TIL/assets/165024639/684aca9f-b31a-4359-86e6-ef79d504e824)

tomcat started on port 8080이 뜨면 서버 구동

![image](https://github.com/Leejieok/TIL/assets/165024639/ffbd4d0d-6d87-4e88-8405-d039c6dea1b2)

Enable annotation processing 체크 해줘야 annotation 사용가능

#### 참고
서블릿은 톰캣 같은 웹 어플리케이션 서버를 직접 설치하고, </br>
그 위에 서블릿 코드를 클래스 파일로 빌드해서 올린 다음, </br>
톰캣 서버를 실행하면 된다. 하지만 이 과정 매우 번거롭다. </br>
스프링 부트는 톰캣 서버를 내장하고 있으므로, 톰캣 서버 설치 없이 편리하게 </br>
서블릿 코드를 실행할 수 있다. </br>

스프링 부트 서블릿 환경 구성 ( 웹 브라우저와 데이터를 주고 받기 위해 request와 response를 가져와야 함 )

```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.ServletComponentScan;

@ServletComponentScan
@SpringBootApplicaion
public class ServletApplication {
  public static void main(String[] args) {
    SpringApplication.run(ServletApplication.class, args);
  }
}

```
@ServletComponentScan
: 스프링이 해당 패키지의 하위 패키지를 모두 찾아 자동으로 서블릿에 등록 (서블릿 자동 등록)

```
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

@WebServlet(name= "helloServlet", urlPatterns = "/hello") //url이 /hello로 오면 해당 클래스가 실행됨.
public class HelloServlet extends HttpServlet {

  @Override
  protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    System.out.println("HelloServlet.service");
    System.out.println("request = " + request);
    System.out.println("response = " + response);

    String username = request.getParameter("username");
    System.out.println("username = " + username);

    response.setContentType("text/plain");
    response.setCharacterEncoding("utf-8");
    response.getWriter().write("hello" + username);
  }
}  

```
서블릿 사용 시 HttpServlet 상속 필수

* @WebServlet 서블릿 어노테이션
  * name: 서블릿 이름
  * urlPatterns: URL매핑
 
HTTP 요청을 통해 매핑된 URL이 호출되면 서블릿 컨테이너는 다음 메서드를 실행한다.
protected void service(HttpServletRequest request, HttpServletResponse response)

* 웹 브라우저 실행
  * http://localhost:8080/hello?username=word
  * 결과: hello world
* 콘솔 실행결과
  	HelloServlet.service
		request = org.apache.catalina.connector.RequestFacade@35d00d49
		response = org.apache.catalina.connector.ResponseFacade@3b17e583
    username = servlet
 
