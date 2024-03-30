# 값 객체와 Aggregate

#### 값 객체(Value Object):

값 객체는 도메인의 개념을 표현하기 위해 사용되지만, 식별성(identity)을 갖지 않는 객체입니다. 즉, 두 개의 값 객체가 동일한 속성 값을 가진다면, 두 객체는 동일한 것으로 간주됩니다. 값 객체는 불변성(Immutable)을 가지며, 객체의 상태를 변경할 때는 새로운 객체를 생성해야 합니다.

값 객체의 예시로는 돈(Money), 좌표(Point), 날짜 범위(DateRange) 등이 있을 수 있습니다. 이러한 객체들은 자체적인 속성 값으로 완전히 정의되며, 그 자체로 의미를 가집니다.

#### Aggregate:

Aggregate는 도메인 모델을 구성하는 요소 중 하나로, 관련된 객체들을 하나의 그룹으로 묶는 개념입니다. Aggregate 내의 객체들은 일관된 변경을 보장받아야 하며, Aggregate 루트를 통해 접근되어야 합니다. Aggregate 루트는 Aggregate의 진입점 역할을 하며, Aggregate에 속한 모든 객체들을 관리합니다.

Aggregate는 도메인의 복잡성을 관리하기 위해 사용됩니다. 예를 들어, '주문(Order)' Aggregate는 '주문 항목(OrderItem)'과 '배송 주소(DeliveryAddress)'를 포함할 수 있으며, 이들은 모두 주문이라는 하나의 일관된 단위 아래에 있습니다.

Aggregate 설계 시 고려할 점:

1. **크기:** 너무 많은 객체를 포함하는 큰 Aggregate는 피해야 합니다. Aggregate는 가능한 한 작아야 하며, 도메인의 규칙과 일관성을 유지하는 데 필요한 최소한의 요소만 포함해야 합니다.
2. **경계:** Aggregate는 도메인의 경계를 명확히 하는 데 도움이 됩니다. 각 Aggregate는 독립적으로 존재할 수 있어야 하며, 다른 Aggregate와의 관계는 ID 참조를 통해 유지됩니다.
3. **일관성:** Aggregate 내의 모든 변경은 일관성을 유지해야 합니다. 이를 위해 Aggregate 루트를 통한 변경만 허용하며, 외부에서는 Aggregate 루트를 통해서만 Aggregate의 상태를 변경할 수 있습니다.

값 객체와 Aggregate는 DDD에서 중요한 역할을 하는데, 값을 표현하는 것이 목적인 경우 값 객체를, 복잡한 도메인 규칙과 일관성이 요구되는 경우 Aggregate를 사용합니다.



## VO 와 Aggregate&#x20;

Spring Boot를 사용하여 DDD의 개념인 값 객체(Value Object)와 집합체(Aggregate)에 대한 예시를 들겠습니다.

#### 값 객체 (Value Object)

값 객체는 개념적으로 완전한 하나의 단위로, 주로 속성에 의해 정의되고, 변경 불가능하며, 식별자를 갖지 않습니다. 값 객체는 다른 객체의 상태를 설명하는 데 사용됩니다. 예를 들어, `Address` 클래스는 값 객체가 될 수 있습니다.

```java
import lombok.Value;
import lombok.Data

@Value // Lombok의 @Value 어노테이션은 불변 객체를 위한 것입니다. @Getter, @AllArgsConstructor, @EqualsAndHashCode, @ToString 등을 포함합니다.
@Data
public class Address {
    String street;
    String city;
    String state;
    String zipCode;
}

```

이 `Address` 객체는 변경 불가능하며, 어떤 사람(Person)의 주소를 설명하기 위해 사용됩니다. 각각의 주소는 그 자체로 완전한 의미를 가지고, 다른 주소와 구별됩니다.

#### 집합체 (Aggregate)

집합체는 연관된 객체의 클러스터로, 일반적으로 하나의 루트 엔티티(aggregate root)를 중심으로 구성됩니다. 이 루트 엔티티는 집합체의 대표로, 집합체의 다른 멤버에 대한 액세스를 제어합니다. 예를 들어, `Order` 클래스는 주문 집합체의 루트 엔티티가 될 수 있으며, 여러 개의 `OrderItem` (값 객체)을 포함할 수 있습니다.

```java
import lombok.Data;
import lombok.NonNull;
import lombok.RequiredArgsConstructor;

@Data // Lombok의 @Data 어노테이션은 @ToString, @EqualsAndHashCode, @Getter, @Setter, @RequiredArgsConstructor 를 모두 포함합니다.
@RequiredArgsConstructor // final 필드 또는 @NonNull 필드에 대한 생성자를 생성합니다.
public class Order {
    private Long id;
    private final List<OrderItem> items = new ArrayList<>();
    private final Address shippingAddress;

    public void addItem(@NonNull Product product, int quantity) {
        this.items.add(new OrderItem(product, quantity));
    }

    // Other methods can be added here if necessary
}

@Data
@RequiredArgsConstructor // final 필드에 대한 생성자를 자동으로 생성합니다.
public class OrderItem {
    private final Product product;
    private final int quantity;
}

```

여기서 `Order` 클래스는 집합체의 루트 엔티티로, 주문에 관련된 모든 정보(주문 항목, 배송 주소 등)를 캡슐화합니다. `OrderItem`은 해당 주문의 일부로서, 특정 상품과 그 수량을 나타냅니다. 집합체 내에서는 `OrderItem` 객체를 통해 주문 항목을 관리하지만, 외부에서는 `Order` 객체를 통해서만 이러한 항목에 접근합니다.

이 예시에서, Spring Boot는 직접적으로 이 객체들을 어떻게 다룰지를 정하지 않습니다. 하지만 Spring Boot 환경에서 이러한 DDD 개념을 구현하여, 리포지토리를 통해 집합체를 영속화하고, 서비스 계층을 통해 비즈니스 로직을 구현할 수 있습니다. Spring Data JPA와 같은 Spring Boot의 기능을 이용하여 이러한 집합체와 값 객체를 데이터베이스와 연동할 수 있습니다.



## Aggregate 와 Entity 차이점

DDD(도메인 주도 설계)에서 애그리게이트는 종종 루트 엔티티를 중심으로 그룹화된 엔티티 및 값 객체의 집합을 의미합니다. 여기서 `Comment` 엔티티는 애그리게이트 루트 역할을 하며, `ContentVo`와 `LikesVo` 값을 내장하여 애그리게이트를 구성합니다.

#### 애그리게이트(Aggregate)와 엔티티(Entity)의 차이:

* **엔티티(Entity)**: 식별자를 통해 구별되는 도메인 객체입니다. 엔티티는 시간이 지나도 그 식별성을 유지합니다.
* **애그리게이트(Aggregate)**: 하나의 루트 엔티티(애그리게이트 루트)와 그와 관련된 모든 엔티티 및 값 객체를 포함하는 클러스터입니다. 애그리게이트는 도메인의 규칙과 일관성을 유지하는 경계 내에서 데이터를 관리합니다.

제공된 코드에서, `Comment` 클래스는 애그리게이트 루트 역할을 합니다. 그 안에 `ContentVo`와 `LikesVo` 값 객체를 포함하고 있으므로, 이는 `Comment` 애그리게이트의 일부로 볼 수 있습니다. `Comment`가 도메인의 규칙과 일관성을 유지하는 데 중심 역할을 하며, 이를 통해 `Comment` 관련 작업이 수행됩니다.

DDD에서 애그리게이트의 경계는 트랜잭션과 일관성의 경계를 정의합니다. 이 경우, `Comment` 애그리게이트는 댓글 생성, 업데이트 등의 작업을 일관된 방식으로 처리하기 위한 경계를 제공합니다.

