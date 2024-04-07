# JPA EntityGraph

`@EntityGraph`는 JPA(Java Persistence API)에서 사용하는 어노테이션으로, 엔터티를 조회할 때 어떤 연관된 엔터티나 속성을 함께 로딩할지 세밀하게 지정할 수 있게 해주는 기능입니다. 이를 통해 JPA의 기본 페치 전략(Fetch Strategy)을 오버라이드할 수 있으며, 특정 쿼리 실행 시 성능 최적화를 달성할 수 있습니다.

#### 기본 개념

* **엔터티 그래프(Entity Graph)**: 엔터티와 그 연관된 엔터티들의 집합입니다. 엔터티 그래프를 사용하여, JPA가 데이터를 로드할 때 어떤 연관 엔터티들을 함께 로드할지 제어할 수 있습니다.
* **페치 타입(Fetch Type)**: 엔터티의 연관된 엔터티를 로딩하는 방법을 결정합니다. `EAGER`는 연관된 엔터티를 즉시 로딩하고, `LAZY`는 연관된 엔터티를 실제로 접근할 때까지 로딩을 지연시킵니다.

#### @EntityGraph 사용법

`@EntityGraph` 어노테이션은 주로 Repository 메서드에 적용됩니다. 이 어노테이션을 사용하여 메서드가 실행될 때 어떤 속성을 기반으로 엔터티 그래프를 적용할지 지정할 수 있습니다. 속성을 명시적으로 지정하거나, `NamedEntityGraph`를 참조할 수도 있습니다.

* **attributePaths**: 함께 로드할 연관 엔터티의 속성 이름을 지정합니다.
* **type**: 페치 타입을 지정합니다. `LOAD`는 지정한 속성들을 EAGER 로딩으로 처리하고, 나머지는 엔터티 클래스에 정의된 대로 처리합니다. `FETCH`는 `attributePaths`에 지정된 속성만 로드합니다.

#### 예시

```java
public interface UserRepository extends JpaRepository<User, Long> {
    // 단순 사용 예
    @EntityGraph(attributePaths = {"department"})
    Optional<User> findById(Long id);

    // NamedEntityGraph 사용 예
    @EntityGraph(value = "User.detail", type = EntityGraph.EntityGraphType.FETCH)
    List<User> findAllByLastName(String lastName);
}
```

#### 장점

* **성능 최적화**: 필요한 데이터만 선택적으로 로딩함으로써 성능을 개선할 수 있습니다.
* **N+1 쿼리 문제 해결**: 적절한 엔터티 그래프를 사용하여 관련 데이터를 한 번의 쿼리로 로드함으로써, N+1 쿼리 문제를 방지할 수 있습니다.
* **유연성**: 기본 페치 전략을 유지하면서, 특정 쿼리에 대해서만 로딩 전략을 변경할 수 있어 유연성이 뛰어납니다.

#### 주의사항

* **오버페칭(Overfetching)**: 필요하지 않은 데이터까지 로딩할 위험이 있어, 성능이 오히려 저하될 수 있습니다. 따라서 사용 시에는 신중한 고려가 필요합니다.
* **복잡성**: 특정 쿼리에 맞춤형으로 로딩 전략을 설정하는 것은 애플리케이션의 복잡성을 증가시킬 수 있으므로, 관리가 필요합니다.

`@EntityGraph`는 데이터 접근 패턴을 최
