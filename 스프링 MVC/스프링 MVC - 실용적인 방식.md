***
## 스프링 MVC - 실용적인 방식.md
MVC 프레임워크 만들기에서 v3은 ModelView를 개발자가 직접 생성해서 반환했기 때문에, 불편했다 </br>
물론 V4를 만들면서 실용적으로 개선했다.

스프링 MVC는 개발자가 편리하게 개발할 수 있도록 수 많은 편의 기능을 제공한다.</br>

## SpringMemberControllerV3
```
package hello.servlet.web.springmvc.v3;

import hello.servlet.domain.member.Member;
import hello.servlet.domain.member.MemberRepository;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

import java.util.List;

/**
 * v3
 * ViewName 직접반환
 * @RequestParam 사용
 * @RequestMapping -> @GetMapping, @PostMapping
 */

@Controller
@RequestMapping("/springmvc/v3/members")
public class SpringMemberControllerV3 {

    private MemberRepository memberRepository = MemberRepository.getInstance();

//    @RequestMapping(value = "/new-form", method = RequestMethod.GET)
    @GetMapping("/new-form")
    public String newForm() {
        return "new-form";
    }

//    @RequestMapping(value = "/save", method = RequestMethod.POST)
    @PostMapping("/save")
    public String save(
            @RequestParam("username") String username,
            @RequestParam("age") int age,
            Model model) {

        Member member = new Member(age, username);
        memberRepository.save(member);

        model.addAttribute("member", member);
        return "save-result";
    }

//    @RequestMapping(method = RequestMethod.GET)
    @GetMapping
    public String members(Model model) {
        List<Member> members = memberRepository.findAll();

        model.addAttribute("members", members);
        return "members";
    }
}
```
**Model 파라미터**
save(), members()를 보면 Model을 파라미터로 받는 것을 확인할 수 있다. 스프링 MVC도 이런 편의 기능을 </br>
제공한다.

**ViewName 직접 반환**
뷰의 논리 이름을 반환할 수 있다.

**@RequestParma 사용**
스프링은 HTTP 요청 파라미터를 @RequestParam으로 받을 수 있다.</br>
@RequestParam("username")은 request.getParameter("username")와 거의 같은 코드라 생각하면 된다.</br>
물론 GET 쿼리 파라미터, POST Form 방식을 모두 지원한다.

**@RequestMapping -> @GetMapping, @PostMapping**
@RequestMapping은 URL만 매칭하는 것이 아니라, HTTP Method도 함께 구분할 수 있다.</br>
예를 들어서 URL이 /new-form이고, HTTP Method가 GET인 경우를 모두 만족하는 매핑을 하려면 다음과 같이 처리하면 된다.</br>
```
@RequestMapping(value = "new-form", method = RequestMethod.GET)
```
이것을 @GetMpping, @PostMapping으로 더 편리하게 사용할 수 있다 </br>
참고로 Get, Post, Put, Delete, Patch 모두 애노테이션이 준비되어 있다.

