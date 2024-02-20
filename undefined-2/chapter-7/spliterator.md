# Spliterator 인터페이스

Iterator 처럼 Spliterator 는 소스의 요소 탐색 기능을 제공한다는 점은 같지만

Spliterator 는 병렬 작업에 특화되어 있다.

Spliterator 를 꼭 직접 구현해야 하는 것은 아니지만 Spliterator 가 어떻게 동작하는지 이해한다면 병렬 스트림 도앚ㄱ 과 관련한 통찰력을 얻을 수 있다.



자바 8 이상은 컬렉션 프레임워크에 포함된 모든 자료구조에 사용할 수 있는 디폴트 Spliterator 구현을 제공합니다.

컬렉션은 spliterator 라는 메서드를 제공하는 Spliterator 인터페이스를 구현합니다.



`Spliterator` 인터페이스는 Java 8에서 도입되었으며, `Iterable` 인터페이스와 비슷한 역할을 하지만 병렬 처리에 특화되어 있습니다. `Spliterator`는 단어 `Split` + `Iterator`의 합성어로, 반복 작업을 수행하면서 필요한 경우에는 데이터를 분할하여 병렬 처리가 가능하게 합니다.

`Spliterator`의 주요 기능은 다음과 같습니다:

1. **분할(Splitting)**: `Spliterator`는 `trySplit()` 메서드를 통해 자신을 부분적으로 분할하여 두 개의 `Spliterator`로 나눌 수 있습니다. 이는 병렬 처리 시에 데이터를 서브셋으로 나누어 여러 스레드가 각자의 서브셋을 처리하도록 할 때 사용됩니다.
2. **크기 추정(Estimation)**: `estimateSize()` 메서드는 남아 있는 요소의 개수를 추정합니다. 이는 Fork/Join 프레임워크가 작업을 얼마나 더 분할할 수 있는지 결정하는 데 도움을 줍니다.
3. **순서(Ordering)**: `Spliterator`는 `ORDERED` 특성을 가질 수 있으며, 이는 반복되는 요소들이 정해진 순서를 가지고 있다는 것을 의미합니다.
4. **특성(Characteristic)**: `characteristics()` 메서드는 `Spliterator`의 특성을 나타내는 값들을 리턴합니다. 이 특성 값들은 `Spliterator`가 `ORDERED`, `DISTINCT`, `SORTED`, `SIZED`, `NONNULL`, `IMMUTABLE`, `CONCURRENT`, `SUBSIZED` 등의 특성을 가지고 있는지를 알려줍니다.
5. **소비(Consumption)**: `tryAdvance()` 메서드를 사용하여 요소들을 하나씩 처리합니다. 이 메서드는 `Iterator`의 `next()`와 `hasNext()`의 역할을 합쳐 놓은 것으로 볼 수 있습니다.

병렬 스트림과의 관계에서 `Spliterator`는 스트림이 병렬로 처리될 때 기본적인 분할 메커니즘을 제공합니다. 병렬 스트림에서는 스트림을 여러 서브스트림으로 분할하고, 각각을 별도의 스레드에서 처리한 다음 결과를 합쳐야 합니다. `Spliterator`는 이러한 분할 작업을 가능하게 해주며, 특히 컬렉션의 크기가 크거나 계산이 복잡할 때 이점을 제공합니다.

예를 들어, `ArrayList`의 병렬 스트림 처리에서는 `ArrayList`가 내부적으로 제공하는 `Spliterator`가 효율적인 분할을 가능하게 해주어 병렬 처리의 성능을 향상시킵니다. 반면 `LinkedList`와 같은 경우는 분할하기 어려워 병렬 스트림에서 그다지 효율적이지 않습니다.

간단히 말해서, `Spliterator`는 데이터 구조를 멀티코어 환경에서 병렬로 처리하기 위한 기본적인 도구로서, 병렬 스트림이 내부적으로 사용하여 성능을 최적화합니다.





<figure><img src="../../.gitbook/assets/스크린샷 2024-02-20 오후 4.20.38.png" alt=""><figcaption></figcaption></figure>
