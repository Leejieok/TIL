***
## 회원 관리 웹 애플리케이션 요구사항

**회원정보**
이름: username
나이: age

**기능 요구사항**
* 회원저장
* 회원 목록 조회

**회원 도메인 모델**
```
import lombok.Getter;
import lombok.Setter;

@Getter @Setter
public class Member {
	
	private Long id;
	private int age;
	private String username;

	public Member( String username, int age) {
		this.username=username;
		this.age=age;
	}

	public Member() {}
}
```
***

1. 매개변수가 있는 생성자: 이 생성자는 객체를 생성할 때 username 과 age를 필수적으로 </br>
받아 초기화 하도록 한다. 이를 통해 생성 시 필요한 필드가 모두 초기화 되도록 보장한다 </br>
2. 기본 생성자: 기본 생성자는 매개변수가 없는 생성자로, 객체를 생성할 때 초기값을 설정하지 </br>
않아도 되도록 한다. 이는 프레임워크가 객체를 생성한 후 setter메서드를 통해 값을 설정할 수 있게한다.</br>

### setter를 통한 값 설정 예제
```
public class Main{
  public static void main(String[] args) {
    Member member = new Member();
    member.setUsername("aaa");
    member.setAge(33);
  }
}
```

### Java에서 public 접근 제한자를 사용하는 이유
1. 객체 생성의 자유로움: 생성자가 public일 경우, 이 클래스를 사용하는 다른 클래스나 외부 코드에서</br>
new Member("username", 25)와 같이 객체를 생성할 수 있다 </br>
2. 의존성 주입과 객체 생성: public 생성자를 사용하면 프레임워크나 라이브러리에서 의존성 주입을 통해 </br>
객체를 생성하고 관리할 수 있다. Spring은 기본적으로 public 생성자를 통해 객체를 생성하고, 필요한 의존성을 주입한다.

***
## MemberRepository
회원 정보 저장소
기능: 들어온 정보를 저장, 기존 회원 조회, 회원 식별 시퀀스 할당

```
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/*
* 동시성 문제가 고려되어 있지 않음, 실무에서는 ConcurrentHashMap, AtomicLong 사용 고려
*/
public class MemberRepository {
	private static Map<Long, Member> store = new HashMap<>();
	private static Long sequence = 0L;

  /*싱글톤 만들 때는 private로 막아주어야 한다. */
	private static final MemberRepository instance = new MemberRepository();

	public static MemberRepository getInstance() {
		return instance;
	}

	private MemberRepository() {}

	public Member save (Member member) { //save를 하는 순간 id 시퀀스가 할당 Member 클래스에 저장되어짐
		member.setId(++sequense);
		store.put(member.getId(), member);
		return member;
	}

	public Member findById(long id) {
		return store.get(id);
	}

	public List<Member> findAll() {
		return new ArrayList<>(store.values());
	}
}
```
