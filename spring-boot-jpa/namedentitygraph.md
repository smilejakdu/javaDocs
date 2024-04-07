# NamedEntityGraph 로 엔터티 그래프 정의

`NamedEntityGraph`는 JPA(Java Persistence API)에서 엔터티의 로딩 전략을 세밀하게 제어할 수 있게 해주는 기능입니다. 이를 사용하면 특정 엔터티를 조회할 때 어떤 연관 엔터티들을 함께 로딩할 것인지, 그리고 그 로딩 방식(EAGER 또는 LAZY)을 미리 정의할 수 있습니다. 이는 성능 최적화에 유용하며, N+1 문제와 같은 흔한 문제를 해결하는 데 도움을 줍니다.

#### 기본 개념

* **엔터티 그래프(Entity Graph)**: 엔터티와 그 연관된 엔터티들의 집합을 말합니다. 이 그래프를 사용하여 JPA가 특정 엔터티를 조회할 때 함께 로딩해야 하는 연관 엔터티들을 지정할 수 있습니다.
* **NamedEntityGraph**: 이름이 지정된 엔터티 그래프로, 엔터티 클래스에 선언하고, 이름을 통해 참조됩니다. 여러 엔터티 그래프를 다른 상황에서 재사용할 수 있게 해줍니다.

#### NamedEntityGraph 선언

`@NamedEntityGraph` 어노테이션을 사용하여 엔터티 클래스에 선언합니다. `name` 속성으로 그래프의 이름을 지정하고, `attributeNodes` 속성으로 함께 로딩할 필드를 지정합니다. 필요에 따라 `includeAllAttributes` 속성을 사용하여 모든 속성을 포함시킬 수도 있습니다.

```java
@Entity
@NamedEntityGraph(name = "Employee.detail",
                  attributeNodes = @NamedAttributeNode("department"))
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @ManyToOne(fetch = FetchType.LAZY)
    private Department department;
    // 생략
}
```

#### 사용 방법

정의한 엔터티 그래프는 `EntityManager` 또는 Spring Data JPA의 Repository에서 사용할 수 있습니다. 예를 들어, Spring Data JPA의 경우 `@EntityGraph` 어노테이션을 메서드에 추가하고, `attributePaths`에 로딩할 연관 필드의 이름을 지정하며, `type`으로 로딩 전략(`LOAD` 또는 `FETCH`)을 지정합니다.

```java
public interface EmployeeRepository extends JpaRepository<Employee, Long> {
    @EntityGraph(attributePaths = {"department"}, type = EntityGraph.EntityGraphType.LOAD)
    Optional<Employee> findById(Long id);
}
```

#### 장점

* **성능 최적화**: 필요한 엔터티만 선택적으로 로딩하여 성능을 최적화할 수 있습니다.
* **N+1 문제 해결**: 적절한 엔터티 그래프를 사용함으로써 쿼리 수를 줄이고, N+1 문제를 방지할 수 있습니다.
* **재사용성**: 이름이 지정된 엔터티 그래프는 애플리케이션 내에서 재사용할 수 있어 코드의 중복을 줄일 수 있습니다.

#### 주의사항

* **성능 저하 가능성**: 잘못 사용된 경우, 필요 이상의 데이터를 로딩하여 오히려 성능 저하를 일으킬 수 있습니다.
* **복잡성 증가**: 로딩 전략을 세밀하게 제어할 수 있는 만큼, 애플리케이션의 복잡성이 증가할 수 있습니다. 따라서 성능 테스트와 분석을 통해 적절한 전략을 선택해야 합니다.
