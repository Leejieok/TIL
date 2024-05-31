***
## 스프링 MVC 
@RequestMapping
* RequestMappingHandlerMapping
* RequestMappingHandlerAdapter

가장 우선순위가 높은 핸들러 매핑과 핸들러 어댑터는 </br>
RequestMappingHandlerMapping, RequestMappingHandlerAdapter 이다. </br>
@RequestMapping의 앞글자를 따서 만든 이름인데, 이것이 지금 스프링에서 주로 사용하는 애너테이션</br>
기반의 컨트롤러를 지원하는 핸들러 매핑과 어댑터이다. </br>

## SpringMemberFormControllerV1
```
package hello.servlet.web.springmvc.v1;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class SpringMemberFormControllerV1 {

    @RequestMapping("/springmvc/v1/members/new-form")
    public ModelAndView process() {
        return new ModelAndView("new-form");
    }

}
```
* `@Controller` :
	* 스프링이 자동으로 스프링 빈으로 등록한다(내부에 @Component 애너테이션이 있어서 컴포넌트 스캔의 대상이 됨)
	* 스프링 MVC에서 애너테이션 기반 컨트롤러로 인식한다.
* `@RequestMapping`: 요청 정보를 매핑한다. 해당 URL이 호출되면 이 메서드가 호출된다. 애너테이션을 기반으로 동작하기 때문에</br>
		메서드의 이름은 임의로 지으면 된다.
* `ModelAndView`: 모델과 뷰 정보를 담아서 반환하면 된다.


**주의 - 스프링 3.0 이상**</br>
스프링 부트 3.0(스프링 프레임워크 6.0)부터는 클래스 레벨에 @RequestMapping이 있어도 스프링</br>
컨트롤러로 인식하지 않는다. 오직 @Controller가 있어야 스프링 컨트롤러로 인식한다. 참고로</br>
@RestController는 해당 애너테이션 내부에 @Controller를 포함하고 있는 것으로 인식 된다. 따라서 </br>
@Controller가 없는 코드는 스프링 컨트롤러로 인식되지 않는다 </br>
(RequestMappingHandlerMapping에서 @RequestMapping은 이제 인식하지 않고, Controller만 인식한다)</br>

@RequestMapping: 이 애노테이션은 메서드 레벨 또는 클래스 레벨에 사용되어 해당 메서드 또는 클래스가 처리할 URL 패턴을 지정한다. </br>
핸들러 매핑은 @RequestMapping이 지정된 메서드를 찾아 해당 요청을 처리할 수 있는 핸들러로 간주한다.</br>

@Component로 스프링 빈 이름으로 핸들러 찾기: 이는 스프링의 컴포넌트 스캔 기능을 통해 @Component 애노테이션이 붙은 </br>
클래스를 스프링 빈으로 등록하고, 해당 빈의 이름을 기반으로 핸들러를 찾는 방식이다.</br>

따라서 @RequestMapping은 핸들러 매핑에 직접적으로 관여하는 애노테이션이다. 하지만 @RequestMapping을 사용하기 위해서는 해당 </br>
클래스가 컨트롤러로 인식되어야 한다. 이를 위해 클래스에 @Controller 또는 @RestController와 같은 애노테이션을 붙여 컨트롤러로 </br>
지정해야 한다.</br>

핸들러 어댑터(HandlerAdapter)는 핸들러 매핑이 찾은 핸들러를 실제로 호출하고 실행하는 역할을 한다. 핸들러 어댑터는 해당 핸들러의 </br>
메서드를 호출하고, 필요한 파라미터를 바인딩하며, 반환값을 처리한다.</br>
@RequestMapping은 핸들러 매핑에 직접적으로 관여하는 애노테이션이다. 핸들러 어댑터는 매핑된 핸들러를 실제로 호출하고 실행하는 역할을 담당함.</br>

```
package hello.servlet.web.springmvc.v1;

import hello.servlet.domain.member.Member;
import hello.servlet.domain.member.MemberRepository;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class SpringMemberSaveControllerV1 {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @RequestMapping("/springmvc/v1/members/save")
    public ModelAndView process(HttpServletRequest request, HttpServletResponse response) throws Exception {
        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(age, username);
        memberRepository.save(member);

        ModelAndView mv = new ModelAndView("save-result");
//        mv.getModel().put("member", member);
        mv.addObject("member", member);
        return mv;
    }
}
```
