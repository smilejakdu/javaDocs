# 필터링

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamFilterExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

        // names 리스트에서 길이가 4보다 큰 이름만 필터링
        List<String> filteredNames = names.stream()
                                          .filter(name -> name.length() > 4)
                                          .collect(Collectors.toList());

        System.out.println(filteredNames); // 결과: [Alice, Charlie]
    }
}

```



## 프레디케이트로 필터링

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class StreamFilterExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

        // Predicate 인터페이스를 사용하여 필터 조건 정의
        Predicate<String> longerThanFour = name -> name.length() > 4;

        // names 리스트에서 길이가 4보다 큰 이름만 필터링
        List<String> filteredNames = names.stream()
                                          .filter(longerThanFour)
                                          .collect(Collectors.toList());

        System.out.println(filteredNames); // 결과: [Alice, Charlie]
    }
}


```

스트림 인터페이스는 filter 메서드를 지원한다.

filter 메서드는 프레디케이트 를 인수로 받아서 프레디케이트와 일치하는 모든 요소를 포함하는 스트림을 반환한다.

## 고유 요소 필터링

스트림은 고유 요소로 이루어진 스트림을 반환하는 distinct 메서드로 지원한다.

```java
List<Integer> numbers = Arrays.asList(1,2,3,3,2,4);
numbers.stream()
    .filter(i -> i % 2 ==0 )
    .distinct()
    .forEach(System.out::println);
```



## 스트림 슬라이싱

스트림의 요소를 선택하거나 스킵하는 다양한 방법을 설명한다. 프레디케이트를 이용하는 방법,

스트림의 처음 몇 개의 요소를 무시하는 방법, 특정 크기로 스트림을 줄이는 방법 등 다양한 방법을 이용해 효율적으로 이런 작업을 수행할 수 있다.



## takeWhile



`takeWhile` 메소드는 스트림의 요소들을 순서대로 처리하면서 주어진 조건(predicate)을 만족하는 동안 요소들을 선택합니다. 조건을 만족하지 않는 첫 번째 요소가 나타나면, 그 시점에서 처리를 중단하고 해당 요소를 포함하지 않은 새로운 스트림을 반환합니다.



조건을 만족하는 요소가 조건을 만족하지 않는 요소 뒤에 위치한다면, `takeWhile`은 이러한 요소들을 무시하고 결과 스트림에서 제외합니다.



```java
import java.util.stream.Stream;
import java.util.List;

public class Example {
    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        Stream<Integer> takenStream = numbers.stream().takeWhile(n -> n < 5);

        takenStream.forEach(System.out::println); // 1, 2, 3, 4를 출력
    }
}

```



위의 코드는 5 미만 숫자들이 모두 출력이 된다 하지만 5미만 숫자들이 뒤에 있다면 어떻게 될까 ??



```java
List<Integer> numbers = List.of(3, 2, 6, 1, 4, 5, 2, 3);

List<Integer> result = numbers.stream()
                              .takeWhile(n -> n < 5)
                              .collect(Collectors.toList());

System.out.println(result);

```

이 경우, 스트림의 처리는 6을 만나는 순간 중단됩니다. 따라서 `result` 리스트에는 `[3, 2]`만 포함됩니다. 6 이후에 나오는 1, 4, 2, 3 등은 조건을 만족함에도 불구하고 `result`에 포함되지 않습니다.

## dropWhile

`dropWhile` 메소드는 주어진 조건을 만족하는 동안 스트림의 요소들을 건너뛰고, 조건을 만족하지 않는 첫 번째 요소부터 나머지 요소들을 포함한 새로운 스트림을 반환합니다.

```java
import java.util.stream.Stream;
import java.util.List;

public class Example {
    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        Stream<Integer> droppedStream = numbers.stream().dropWhile(n -> n < 5);

        droppedStream.forEach(System.out::println); // 5, 6, 7, 8, 9, 10을 출력
    }
}

```



이 예제에서 `dropWhile`은 5 미만의 숫자들을 모두 건너뛰고, 5 이상의 숫자들로 구성된 새로운 스트림을 생성합니다.

#### 주의 사항

* `takeWhile`과 `dropWhile`은 스트림이 정렬되어 있을 때 가장 예측 가능하게 동작합니다. 스트림이 정렬되어 있지 않다면, 조건을 만족하는 요소가 뒤에 또 나타날 수 있음에도 불구하고 처리가 중단될 수 있습니다.
* 이 메소드들은 주로 무한 스트림이나 큰 데이터셋을 다룰 때 유용하게 사용될 수 있습니다.



