# DDD 개념

도메인 주도 설계(DDD: Domain-Driven Design)는 복잡한 요구사항을 가진 소프트웨어 프로젝트를 위한 접근 방식입니다. 이 접근 방식은 복잡한 소프트웨어의 설계를 도메인의 복잡성에 중점을 두고 진행하는 것을 목표로 합니다. 도메인은 소프트웨어가 해결하려고 하는 비즈니스 문제 영역입니다.

DDD에서는 주로 다음과 같은 구성 요소들을 사용합니다:

1. **엔티티(Entity):** 식별자를 가지고 동등성이 결정되는 도메인 객체.
2. **값 객체(Value Object):** 식별자를 가지지 않고, 그 속성들로만 동등성이 결정되는 도메인 객체.
3. **집합(Aggregate):** 하나의 엔티티를 루트로 하는 관련 엔티티와 값 객체의 묶음.
4. **리포지토리(Repository):** 집합의 영속성을 관리하는 객체.
5. **서비스(Service):** 도메인 로직을 수행하지만 도메인 모델 내에 자연스럽게 위치할 수 없는 로직을 포함하는 객체.

Spring Boot에서 DDD를 적용하는 예를 들어보겠습니다. 여기서는 간단한 `User` 도메인을 예로 들겠습니다.

**엔티티(Entity):**

```java
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    private String name;
    private String email;

    // Constructors, Getters and Setters
}
```

**리포지토리(Repository):**

```java
public interface UserRepository extends JpaRepository<User, Long> {
    User findByEmail(String email);
}
```

**서비스(Service):**

```java
@Service
@RequiredArgsConstructor
public class UserService {

    private final UserRepository userRepository;

    public User registerUser(String name, String email) {
        User newUser = new User();
        newUser.setName(name);
        newUser.setEmail(email);
        return userRepository.save(newUser);
    }

    // Other business methods
}
```

**컨트롤러(Controller):**

```java
@RestController
@RequiredArgsConstructor
public class UserController {

    private final UserService userService;

    @PostMapping("/users")
    public User registerUser(@RequestBody UserDto userDto) {
        return userService.registerUser(userDto.getName(), userDto.getEmail());
    }

    // Other endpoints
}
```

이 예에서 `User` 클래스는 엔티티입니다. `UserRepository`는 리포지토리로서 `User` 엔티티의 영속성을 관리합니다. `UserService`는 도메인 로직을 포함하고 있으며, `UserController`는 외부에서 도메인 로직에 접근할 수 있게 해주는 엔드포인트를 제공합니다.

DDD를 적용하면 코드의 구조가 더 명확해지고, 도메인 로직이 더 집중되며, 비즈니스 요구사항의 변화에 더 유연하게 대응할 수 있습니다.
