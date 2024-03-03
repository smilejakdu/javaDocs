# 마블 다이어그램으로 이해하는 Publisher

{% embed url="https://velog.io/@wnguswn7/Section4-7.-Spring-Webflux-%EB%A6%AC%EC%95%A1%ED%8B%B0%EB%B8%8C-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D" %}



<figure><img src="../.gitbook/assets/스크린샷 2024-03-03 오후 10.24.58.png" alt=""><figcaption></figcaption></figure>



## Web Flux 의 메서드



`Flux`는 다양한 메서드를 제공하여 데이터 스트림의 생성, 변환, 조합, 소비 등 다양한 작업을 수행할 수 있게 합니다. 여기 몇 가지 중요한 `Flux` 메서드에 대한 설명을 드리겠습니다:

#### 데이터 스트림 생성:

* **just(T... values)**: 하나 이상의 요소로 `Flux` 시퀀스를 생성합니다.
* **fromIterable(Iterable\<? extends T> iterable)**: `Iterable`로부터 `Flux` 시퀀스를 생성합니다.
* **range(int start, int count)**: 지정된 범위의 정수를 포함하는 `Flux`를 생성합니다.
* **empty()**: 아무 요소도 포함하지 않는 `Flux`를 생성합니다.
* **error(Throwable error)**: 지정된 오류를 방출하는 `Flux`를 생성합니다.
* **fromStream(Stream\<? extends T> stream)**: Java의 `Stream`으로부터 `Flux`를 생성합니다.

#### 변환과 필터링:

* **map(Function\<? super T, ? extends V> mapper)**: 각 요소를 주어진 함수로 변환합니다.
* **flatMap(Function\<? super T, ? extends Publisher\<? extends V>> mapper)**: 각 요소를 다른 `Publisher`로 변환하고, 그 결과를 하나의 `Flux`로 플랫하게 만듭니다.
* **filter(Predicate\<? super T> predicate)**: 주어진 조건에 맞는 요소만을 포함하는 새로운 `Flux`를 생성합니다.
* **distinct()**: 중복 값을 제거한 새로운 `Flux`를 생성합니다.

#### 에러 처리:

* **onErrorReturn(T fallbackValue)**: 오류가 발생했을 때 지정된 값으로 대체합니다.
* **onErrorResume(Function\<? super Throwable, ? extends Publisher\<? extends T>> fallback)**: 오류가 발생했을 때 다른 `Publisher`로 대체합니다.
* **retry(long times)**: 오류가 발생했을 때 지정된 횟수만큼 재시도합니다.

#### 조합:

* **mergeWith(Publisher\<? extends T> other)**: 다른 `Publisher`와 현재 `Flux`의 항목들을 병합하여 순서에 상관없이 방출합니다.
* **concatWith(Publisher\<? extends T> other)**: 다른 `Publisher`와 현재 `Flux`를 연결하여 순서대로 방출합니다.
* **zipWith(Publisher\<? extends T> other)**: 두 `Publisher`의 항목들을 튜플로 묶어서 방출합니다.

#### 소비:

* **subscribe()**: 데이터 스트림을 소비하고, 요소를 방출할 준비가 됩니다. 구독자를 추가하지 않으면 아무 동작도 수행하지 않습니다.
* **blockFirst()**: `Flux`에서 첫 번째 요소를 방출할 때까지 블록합니다. (주로 테스트에 사용됨)
* **blockLast()**: `Flux`에서 마지막 요소를 방출할 때까지 블록합니다. (주로 테스트에 사용됨)

이러한 메서드는 `Flux`를 사용하여 복잡한 비동기 데이터 스트림을 손쉽게 조작하고 처리할 수 있도록 도와줍니다. Reactor 패러다임을 따르기 때문에, 이러한 메서드들을 체이닝하여 사용할 수 있으며, 리액티브 프로그래밍의 풍부한 기능을 제공합니다.

## Flux 예제&#x20;



```java
public class FluxExample01 {
    public static void main(String[] args) {
        Flux.just(6, 9, 13)
            .map(num -> num % 2)
            .subscribe(remainder -> Logger.info("# remainder: {}", remainder));
    }
}

```

1. `Flux.just(6,9,13)`: 이 부분에서는 세 개의 정수(6, 9, 13)를 포함하는 `Flux` 인스턴스를 생성합니다. `Flux.just()` 메서드는 주어진 요소들로 `Flux` 시퀀스를 생성합니다.
2. `.map(num -> num % 2)`: `map` 연산자는 `Flux` 스트림의 각 요소에 함수를 적용합니다. 여기서는 각 숫자(`num`)를 2로 나눈 나머지를 계산합니다. 즉, 각 숫자를 홀수인지 짝수인지를 판별하는 값(0 또는 1)으로 변환합니다.
3. `.subscribe(remainder -> Logger.info("# remainder: {}", remainder)`: `subscribe` 메서드는 생성된 `Flux` 스트림을 소비하는데 사용됩니다. 여기서는 각 요소(여기서는 각 숫자의 2로 나눈 나머지)에 대해 로깅을 수행합니다. `Logger.info`는 로깅 프레임워크를 통해 로그 메시지를 출력하는 방법을 제공합니다. 각 요소의 나머지가 `{}` 위치에 삽입되어 로그 메시지로 기록됩니다.

```java
public class FluxExample01 {
    public static void main(String[] args) {
        Flux.fromArray(new Interger[] {3,6,7,9})
            .filter(num -> num > 6)
            .map(num -> num * 2)
            .subscribe(multiply -> Logger.info("# multiply: {}", multiply));
    }
}
```

이 Java 프로그램은 Project Reactor의 `Flux`를 사용하여 리액티브 프로그래밍 개념을 보여줍니다. 코드의 주요 목적과 각 단계를 설명하겠습니다:

1.  **Flux 생성**:

    ```java
    Flux.fromArray(new Integer[] {3,6,7,9})
    ```

    이 라인은 배열 `{3, 6, 7, 9}`에서 데이터를 가져와서 `Flux` 시퀀스를 생성합니다. 이 `Flux`는 네 개의 정수 값을 포함합니다.
2.  **필터링**:

    ```java
    .filter(num -> num > 6)
    ```

    이 `filter` 연산자는 `Flux` 시퀀스를 통과하는 각 숫자에 대해 조건을 적용합니다. 여기서는 6보다 큰 숫자만 통과시킵니다. 따라서, 원래의 네 개의 숫자 중에서 7과 9만 다음 단계로 넘어갑니다.
3.  **매핑**:

    ```java
    .map(num -> num * 2)
    ```

    이 `map` 연산자는 필터링된 스트림의 각 요소에 함수를 적용합니다. 여기서는 각 숫자에 2를 곱합니다. 따라서, 7과 9는 각각 14와 18로 변환됩니다.
4.  **구독 및 출력**:

    ```java
    .subscribe(multiply -> Logger.info("# multiply: {}", multiply));
    ```

    `subscribe` 메서드는 `Flux` 스트림의 최종 소비자입니다. 이 경우, 스트림의 각 요소에 대해 로그 메시지를 출력하는 람다 표현식이 있습니다. 로그 메시지에는 변환된 각 숫자(14와 18)가 포함됩니다.

주의해야 할 점은 `Logger.info("# multiply: {}", multiply)` 부분에서 실제 Java 코드에서 `Logger`가 적절히 설정되어 있어야 하며, SLF4J 같은 로깅 프레임워크의 일부일 것입니다. 만약 `Logger` 설정이 없거나 잘못 설정되어 있다면, 이 부분에서 컴파일 오류나 런타임 오류가 발생할 수 있습니다. 실제로 이 코드를 실행하기 전에 로거의 설정과 초기화가 필요할 수 있습니다.

이 코드의 전반적인 목적은 주어진 배열에서 특정 조건(6보다 큰)에 맞는 요소를 찾아, 그 값을 변형(두 배로)하고, 그 결과를 로그로 출력하는 것입니다. 이는 리액티브 프로그래밍의 일반적인 패턴을 따르며, 비동기 및 비차단 방식으로 동작할 수 있습니다.



```java
public class FluxExample01 {
    public static void main(String[] args) {
        Flux<Objec> flux =
            Mono.justOrEmpty(null)
                .concatWith(Mono.justOrEmpty("Jobs"));
        flux.subscribe(data -> Logger.info("# result : {}", data));
    }
}
```



이 코드는 Project Reactor를 사용하여 리액티브 프로그래밍의 개념을 보여주는 예제입니다. 코드의 주요 목적과 각 단계를 아래에 설명하겠습니다:

1.  **Mono 생성**:

    ```java
    Mono.justOrEmpty(null)
    ```

    이 부분에서 `Mono.justOrEmpty` 메소드는 `null` 값을 받습니다. 이 메소드는 주어진 값이 `null`인 경우 빈 `Mono`를 생성하고, 그렇지 않은 경우 해당 값으로 `Mono`를 생성합니다. 여기서는 `null`이 주어졌으므로, 결과는 빈 `Mono`입니다.
2.  **Mono 결합**:

    ```java
    .concatWith(Mono.justOrEmpty("Jobs"))
    ```

    다음으로, `concatWith` 메소드를 사용하여 두 개의 `Mono`를 연결합니다. 첫 번째 `Mono`는 빈 `Mono`이고, 두 번째 `Mono`는 "Jobs" 문자열을 포함합니다. `concatWith`는 첫 번째 `Mono`가 완료된 후 두 번째 `Mono`의 데이터를 방출합니다. 여기서 첫 번째 `Mono`는 빈 상태이므로, 실질적으로 "Jobs"가 포함된 두 번째 `Mono`만 방출됩니다.
3.  **Flux 생성**:

    ```java
    Flux<Object> flux = ...
    ```

    여기서 변수 `flux`는 위에서 생성된 두 `Mono`의 결합 결과를 참조합니다. 이는 `Flux<Object>` 타입으로 선언되어 있으며, 여기서는 "Jobs" 문자열 하나만 포함하게 됩니다. (원래 코드에서는 `Flux<Objec>`라고 되어 있지만, 이것은 오타로 보이며 `Flux<Object>`가 올바른 표현입니다.)
4.  **구독 및 출력**:

    ```java
    flux.subscribe(data -> Logger.info("# result : {}", data));
    ```

    `subscribe` 메서드는 `Flux` 스트림의 최종 소비자입니다. 여기서는 스트림의 각 데이터 항목에 대해 로그 메시지를 출력하는 람다 표현식을 사용합니다. 이 경우 "Jobs" 문자열이 로그에 출력됩니다.

이 코드의 전반적인 목적은 `null` 값으로 시작하는 `Mono`와 "Jobs" 문자열을 포함하는 또 다른 `Mono`를 결합하여 결과 데이터 스트림을 생성하고, 이 스트림에서 데이터가 방출될 때마다 로그 메시지를 출력하는 것입니다. 주의할 점은 실제 로깅을 위해서는 `Logger` 객체가 초기화되고 설정되어 있어야 하며, 이 예제에서는 로깅 설정에 대한 세부 정보가 제공되지 않았습니다.



```java
public class FluxExample01 {
    public static void main(String[] args) {
        Flux.concat(
            Flux.just("venus")
            Flux.just("Earth")
            Flux.just("Mars")
        .collectList()
        .subscribe(planetList -> Logger.info("# Solar System: {}", planetList));
    }
}
```



이 코드는 Java로 작성된 간단한 프로그램으로, Project Reactor의 `Flux`를 사용하여 리액티브 프로그래밍 개념을 보여줍니다. 그러나 제공된 코드에는 몇 가지 문법적 오류가 있습니다. 올바른 구문을 사용한 후에 코드의 작동 방식을 설명하겠습니다.

```java
import reactor.core.publisher.Flux;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class FluxExample01 {
    private static final Logger Logger = LoggerFactory.getLogger(FluxExample01.class);

    public static void main(String[] args) {
        Flux.concat(
            Flux.just("Venus"),
            Flux.just("Earth"),
            Flux.just("Mars")
        )
        .collectList()
        .subscribe(planetList -> Logger.info("# Solar System: {}", planetList));
    }
}
```

이제 각 부분에 대해 설명하겠습니다:

1.  **Flux 생성 및 결합**:

    ```java
    Flux.concat(
        Flux.just("Venus"),
        Flux.just("Earth"),
        Flux.just("Mars")
    )
    ```

    이 부분에서 `Flux.concat()` 메서드는 여러 `Flux` 인스턴스를 순차적으로 결합합니다. 여기서는 세 개의 `Flux`가 있으며, 각각 "Venus", "Earth", "Mars" 문자열을 포함합니다. `concat` 연산은 이 세 개의 스트림을 순서대로 결합하여, 하나의 연속된 스트림을 생성합니다.
2.  **리스트로 수집**:

    ```java
    .collectList()
    ```

    `collectList()` 메서드는 스트림의 모든 항목을 수집하여 단일 리스트로 변환합니다. 여기서는 "Venus", "Earth", "Mars"가 포함된 리스트 하나를 생성합니다.
3.  **구독 및 로깅**:

    ```java
    .subscribe(planetList -> Logger.info("# Solar System: {}", planetList));
    ```

    `subscribe()` 메서드는 최종적으로 스트림의 결과를 소비합니다. 여기서는 생성된 행성 리스트를 로깅합니다. 이 람다 함수는 리스트가 준비되면 실행되며, 리스트의 내용을 로그로 출력합니다. 출력될 로그 메시지는 행성 리스트를 포함할 것입니다.

이 코드의 전반적인 목적은 "Venus", "Earth", "Mars" 문자열을 포함하는 세 개의 `Flux` 스트림을 결합하고, 이를 하나의 리스트로 변환한 다음, 그 결과를 로깅하는 것입니다. 결과적으로, 우리 태양계의 세 행성 이름이 포함된 리스트가 로그에 기록됩니다.

