# Slf4j

`@Slf4j`는 Project Lombok 라이브러리에서 제공하는 어노테이션으로, Java 클래스에 로깅(Logging) 기능을 쉽게 추가할 수 있도록 해줍니다. 이 어노테이션을 클래스 위에 선언하면, Lombok이 해당 클래스에 자동으로 `log`라는 이름의 로깅 객체를 생성해줍니다. 이 로그 객체는 SLF4J(Simple Logging Facade for Java) 로깅 파사드를 사용하여 실제 로깅 구현체와 통신합니다.

`@Slf4j` 어노테이션을 사용하면 다음과 같은 장점이 있습니다:

1. **코드 간결성**: 별도의 로거 객체를 수동으로 선언하고 인스턴스화할 필요 없이, Lombok 어노테이션 하나로 로깅 기능을 클래스에 추가할 수 있습니다. 이로 인해 코드가 더 간결해지고 읽기 쉬워집니다.
2. **표준화된 로깅 접근 방식**: `@Slf4j`를 사용함으로써 프로젝트 내에서 일관된 로깅 접근 방식을 유지할 수 있습니다. 모든 로그 호출이 `log` 객체를 통해 이루어지므로, 로깅 방식에 일관성을 가질 수 있습니다.
3. **유연성**: SLF4J는 로깅 파사드이므로, 실제 로깅 구현체(예: Logback, log4j)를 프로젝트 설정을 통해 쉽게 교체할 수 있습니다. 이는 코드 변경 없이 로깅 행위를 커스터마이즈할 수 있음을 의미합니다.

`@Slf4j`를 사용한 클래스 예시는 다음과 같습니다:

```java
import lombok.extern.slf4j.Slf4j;

@Slf4j
public class SomeClass {
    public void someMethod() {
        log.info("Some information");
        log.error("An error occurred", new RuntimeException("Error"));
    }
}
```

위 예시에서는 `@Slf4j` 어노테이션이 클래스에 추가되어 있으며, 이로 인해 `log` 객체를 통해 다양한 로그 레벨(info, error, debug 등)에서 메시지를 기록할 수 있습니다.
