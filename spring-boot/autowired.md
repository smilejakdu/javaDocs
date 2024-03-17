# Autowired



`@Autowired`는 Spring Framework에서 제공하는 주입(Injection) 어노테이션입니다. 이 어노테이션은 Spring이 관리하는 빈(Bean)을 자동으로 주입해주는 역할을 합니다. 빈은 Spring IoC(Inversion of Control) 컨테이너에 의해 관리되는 객체를 의미하며, 이들은 보통 `@Component`, `@Service`, `@Repository`, `@Controller` 등의 어노테이션으로 정의됩니다.

`@Autowired`를 사용하면, 개발자가 수동으로 의존성을 주입할 필요 없이, Spring Framework가 필요한 객체를 알아서 찾아서 주입해줍니다. 이는 의존성 주입(Dependency Injection) 원칙을 따르며, 코드의 결합도를 낮추고 유연성을 높이는 데 도움을 줍니다.

#### @Autowired 사용 예시:

```java
@Service
public class SomeService {
    private final SomeRepository someRepository;

    @Autowired
    public SomeService(SomeRepository someRepository) {
        this.someRepository = someRepository;
    }
}
```

위 예시에서 `SomeService` 클래스는 `SomeRepository`에 대한 의존성을 가지고 있습니다. `@Autowired` 어노테이션을 생성자에 붙임으로써, Spring은 `SomeRepository` 타입에 맞는 빈을 컨테이너에서 찾아 `SomeService` 인스턴스 생성 시 자동으로 주입합니다.

#### @Autowired의 작동 방식:

1. **타입에 의한 자동 연결**: Spring은 `@Autowired`가 붙은 필드, 생성자, 또는 메서드의 파라미터 타입을 기반으로 해당 타입의 빈을 컨테이너에서 찾아 자동으로 주입합니다.
2. **생성자 주입**: 생성자에 `@Autowired`를 사용하면, 해당 클래스의 인스턴스를 생성할 때 필요한 의존성을 모두 주입받게 됩니다. Spring 4.3 이후부터는 단일 생성자의 경우 `@Autowired`를 생략할 수 있습니다.
3. **필드 주입**: 클래스 내의 필드에 직접 `@Autowired`를 사용할 수 있으나, 이 방식은 변경 불가능한(immutable) 의존성을 갖기 어렵고, 테스트가 더 복잡해질 수 있어 권장되지 않습니다.
4. **메서드 주입**: 세터 메서드나 일반 메서드에 `@Autowired`를 사용하여 의존성을 주입받을 수 있습니다. 이 방식은 필요에 따라 선택적으로 의존성을 주입받고자 할 때 유용할 수 있습니다.

#### 주의사항:

* 순환 참조 문제: 두 개 이상의 빈이 서로를 참조하고 있을 경우, `@Autowired`를 사용한 순환 참조는 애플리케이션의 시작에 실패할 수 있습니다.
* 올바른 빈 타입의 중요성: `@Autowired`로 주입받으려는 타입의 빈이 없거나, 같은 타입의 빈이 여러 개 있는 경우 (빈 이름으로 구분하지 않는 한), Spring은 예외를 발생시킵니다. 이런 경우 `@Qualifier` 어노테이션을 사용하여 주입받을 빈을 명시적으로 지정할 수 있습니다.

