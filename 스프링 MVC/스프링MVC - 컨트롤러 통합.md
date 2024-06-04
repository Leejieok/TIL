***
## 스프링MVC - 컨트롤러 통합
@RequestMapping을 잘보면 클래스 단위가 아니라 메서드 단위에 적용된 것을 확인할 수 있다. 따라서 컨트롤러 클래스를 </br>
유연하게 하나로 통합할 수 있다.

## SpringMemberControllerV2
```
package hello.servlet.web.springmvc.v2;

import hello.servlet.domain.member.Member;
import hello.servlet.domain.member.MemberRepository;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import java.util.List;

@Controller
public class SpringMemberControllerV2 {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @RequestMapping("/springmvc/v2/members/new-form")
    public ModelAndView newForm() {
        return new ModelAndView("new-form");
    }

    @RequestMapping("/springmvc/v2/members/save")
    public ModelAndView save(HttpServletRequest request, HttpServletResponse response) throws Exception {
        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(age, username);
        memberRepository.save(member);

        ModelAndView mv = new ModelAndView("save-result");
//        mv.getModel().put("member", member);
        mv.addObject("member", member);
        return mv;
    }

    @RequestMapping("/springmvc/v2/members")
    public ModelAndView members() {
        List<Member> members = memberRepository.findAll();

        ModelAndView mv = new ModelAndView("members");

        mv.addObject("member", members);
        return mv;
    }
}
```
### 조합
컨트롤러 클래스를 통합하는 것을 넘어서 조합도 가능하다. </br>
다음 코드는 /springmvc/v2/members 라는 부분에 중복이 있다 </br>
* @RequestMapping("/springmvc/v2/members/new-form")
* @RequestMapping("/springmvc/v2/members")
* @RequestMapping("/springmvc/v2/members/save")

클래스 레벨에 다음과 같이 @RequestMapping을 두면 메서드 레벨과 조합이 된다.
```
@Controller
@RequestMapping("/springmvc/v2/members")
public class SpringMemberControllerV2 {}
```
**조합 결과**
* 클래스 레벨 @RequestMapping("/springmvc/v2/members")
  * 메서드 레벨 @RequestMapping("/new-form") -> /springmvc/v2/members/new-form
  * 메서드 레벨 @RequestMapping("/save") -> /springmvc/v2/members/save
  * 메서드 레벨 @RequestMapping -> /springmvc/v2/members/new-form
