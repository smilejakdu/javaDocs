# JPA EntityGraph

`@EntityGraph`는 Java Persistence API(JPA)에서 엔터티의 로딩 전략을 쉽게 변경할 수 있도록 해주는 어노테이션입니다. 이를 사용하면 엔터티를 조회할 때 어떤 연관된 엔터티나 컬렉션을 EAGER 또는 LAZY 로딩 방식으로 함께 가져올지 세밀하게 제어할 수 있습니다. `@EntityGraph`는 주로 Repository 메서드 레벨에서 사용되며, 특정 조회 작업에 대해 표준 JPA의 로딩 전략을 오버라이드하고자 할 때 유용합니다.

#### @EntityGraph의 주요 속성

* **name**: `@NamedEntityGraph`에 정의된 엔터티 그래프의 이름을 지정합니다. 이 속성을 사용하면 엔터티 클래스에 미리 정의된 그래프를 참조할 수 있습니다.
* **attributePaths**: 로딩할 연관 필드의 이름을 지정합니다. `name` 속성을 사용하지 않을 때 이 속성으로 직접 로딩할 필드를 정의할 수 있습니다.
* **type**: `EntityGraphType.LOAD`와 `EntityGraphType.FETCH` 중 하나를 선택하여 로딩 방식을 지정합니다. `LOAD`는 지정된 속성을 EAGER 로딩으로 처리하고, 나머지는 엔터티 클래스에 정의된 기본 전략을 따릅니다. `FETCH`는 지정된 속성과 클래스에 정의된 EAGER 속성만 EAGER 로딩으로 처리하고, 나머지는 LAZY 로딩으로 처리합니다.

#### 사용 예시

**Repository에서 @EntityGraph 사용하기**

```java
public interface BookRepository extends JpaRepository<Book, Long> {
    @EntityGraph(attributePaths = {"author"})
    List<Book> findAllBy();
}
```

위 예시에서 `findAllBy` 메서드는 `Book` 엔터티를 조회할 때 연관된 `author` 필드를 EAGER 로딩으로 함께 가져옵니다. JPA의 기본 로딩 전략은 LAZY이지만, `@EntityGraph`를 사용하여 이를 오버라이드한 것입니다.

**NamedEntityGraph와 함께 사용하기**

먼저 엔터티 클래스에 `@NamedEntityGraph`를 정의합니다:

```java
@Entity
@NamedEntityGraph(name = "Book.detail",
                  attributeNodes = @NamedAttributeNode("author"))
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    private Author author;
    // 생략
}
```

그리고 Repository에서 해당 그래프를 참조합니다:

```java
public interface BookRepository extends JpaRepository<Book, Long> {
    @EntityGraph(value = "Book.detail", type = EntityGraph.EntityGraphType.LOAD)
    Optional<Book> findById(Long id);
}
```

이 예시에서 `findById` 메서드는 `Book.detail` 엔터티 그래프를 사용하여 `Book` 조회 시 `author`를 EAGER 로딩으로 함께 가져옵니다.
