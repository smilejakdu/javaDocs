# Domain & Entity

## 순환 참조

Join 을 사용하다보면 순환 참조가 걸리는 경우가 있다.

제공된 코드를 바탕으로 보면, `Board`와 `User` 엔티티 사이에는 `@ManyToOne`과 `@OneToMany`로 양방향 관계가 설정되어 있습니다. 또한, `User`와 `Review`, `Board`와 `Review` 사이에도 양방향 관계가 설정되어 있습니다. 이러한 양방향 매핑은 데이터 관계를 표현하는 데 매우 유용하지만, JSON으로 변환할 때 무한 재귀 문제를 일으킬 수 있습니다.

양방향 관계에서 한 쪽이 다른 쪽을 참조하고 그 참조된 쪽이 다시 원래 쪽을 참조하는 상황에서는, JSON 직렬화 과정에서 무한 루프에 빠질 수 있습니다. 이것이 바로 `StackOverflowError`가 발생하는 주된 이유입니다.

문제 해결을 위한 방안은 다음과 같습니다:

1.  **@JsonManagedReference, @JsonBackReference 사용**: 이 어노테이션들은 Jackson 라이브러리에서 제공하며, JSON 직렬화 시 무한 재귀를 방지합니다. 예를 들어, `User` 엔티티의 `boards` 필드에 `@JsonManagedReference`를 추가하고, `Board` 엔티티의 `user` 필드에 `@JsonBackReference`를 추가할 수 있습니다. 이것은 서로 참조하는 객체 간의 무한 재귀를 방지합니다.

    ```java
    // User 클래스 내
    @JsonManagedReference
    private List<Board> boards;

    // Board 클래스 내
    @JsonBackReference
    private User user;
    ```
2. **@JsonIgnore 사용**: 특정 필드의 직렬화를 완전히 무시하려면 `@JsonIgnore` 어노테이션을 사용할 수 있습니다. 하지만 이 방법을 사용하면 해당 필드는 JSON 응답에서 완전히 제외됩니다.
3.  **@JsonIdentityInfo 사용**: 이 어노테이션은 엔티티의 식별자를 사용하여 참조를 처리합니다. 이 방법은 객체 간의 관계를 유지하면서도 무한 재귀 문제를 해결할 수 있습니다.

    ```java
    @JsonIdentityInfo(
      generator = ObjectIdGenerators.PropertyGenerator.class, 
      property = "id"
    )
    public class User {
        // 클래스 내용
    }

    @JsonIdentityInfo(
      generator = ObjectIdGenerators.PropertyGenerator.class, 
      property = "id"
    )
    public class Board {
        // 클래스 내용
    }
    ```

이러한 변경을 통해 `StackOverflowError` 문제를 해결할 수 있으며, 양방향 관계에서도 안전하게 JSON 직렬화를 수행할 수 있습니다.
