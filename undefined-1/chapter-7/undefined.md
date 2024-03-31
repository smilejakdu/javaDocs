# 병렬 스트림 효과적으로 사용하기

병렬 스트림이라고 해서 작업해야할 리소스가 많을때 무조건 적으로 구현하게 되면 안된다.

공유된 데이터일 경우 조심해야한다.



1. 확신이 들지 않으면 직접 측정하는것도 방법이다.&#x20;
2. limit 나 findFirst 처럼 요소의 순서에 의존하는 연산을 병렬 스트림에서 수행하려면 비용이 비싸다 이럴경우 findAny 메서드를 사용하는것이 오히려 비용이 저렴하다.
3. 소량의 데이터에서는 병렬 스트림이 도움이 되지않는다.



그러므로

1. 대량의 데이터이다.
2. 데이터의 순서가 중요하지 않다.

일때 사용하면 좋다



## 병렬스트림 ArrayList vs LinkedList

ArrayList 는 병렬 스트림에서 좋은 성능을 보이는 반면, LinkedList는 병렬 스트림에서 좋은 성능을 보이지 않습니다



ArrayList는 병렬 스트림에서 좋은 성능을 보이는 반면, LinkedList는 병렬 스트림에서 좋은 성능을 보이지 않는다고 합니다. 또한, 병렬 스트림을 사용할 때 내부적으로 사용되는 `Spliterator`에 대한 설명도 포함되어 있는 것 같습니다.

병렬 스트림은 자바 8부터 도입된 기능으로, 스트림의 연산을 멀티 쓰레드 환경에서 동시에 수행할 수 있게 해줍니다. 이를 통해 데이터가 큰 컬렉션을 처리할 때 성능 이점을 얻을 수 있습니다. 하지만 모든 경우에 병렬 스트림이 더 좋은 성능을 보이는 것은 아니며, 사용하는 데이터 구조와 작업의 종류에 따라서 달라집니다.

병렬 스트림을 사용하는 간단한 자바 코드 예시는 다음과 같습니다:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class ParallelStreamExample {
    public static void main(String[] args) {
        // ArrayList with a large number of elements
        List<Integer> numberList = IntStream.rangeClosed(1, 1000000)
                                             .boxed()
                                             .collect(Collectors.toCollection(ArrayList::new));
        
        // Sequential stream sum
        long startTime = System.currentTimeMillis();
        long sum = numberList.stream().reduce(0, Integer::sum);
        long endTime = System.currentTimeMillis();
        System.out.println("Sequential stream sum: " + sum);
        System.out.println("Time taken: " + (endTime - startTime) + " ms");

        // Parallel stream sum
        startTime = System.currentTimeMillis();
        sum = numberList.parallelStream().reduce(0, Integer::sum);
        endTime = System.currentTimeMillis();
        System.out.println("Parallel stream sum: " + sum);
        System.out.println("Time taken: " + (endTime - startTime) + " ms");
    }
}
```

위의 코드는 1부터 1000000까지의 숫자를 포함하는 ArrayList에서 순차 스트림과 병렬 스트림을 사용하여 합계를 계산하는 예시입니다. 실행 시간을 측정하여 병렬 스트림이 실제로 성능 향상을 제공하는지 확인할 수 있습니다.

LinkedList에서 병렬 스트림을 사용하는 것은 권장되지 않는데, LinkedList는 요소 접근이 순차적으로 이루어지기 때문에 데이터를 여러 쓰레드에 분배하는데 비효율적입니다. 반면, ArrayList는 인덱스를 통한 빠른 임의 접근이 가능하여 병렬 처리에 더 적합합니다.

위 코드를 실행하여 병렬 스트림의 성능을 실제로 확인해볼 수 있습니다. 단, 실제 어플리케이션에서 병렬 스트림을 사용하기 전에는 해당 작업에 대한 성능 테스트를 수행하여 실제로 이득이 있는지 검증하는 것이 중요합니다.
