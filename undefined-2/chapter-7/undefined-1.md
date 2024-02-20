# 포크 조인 프레임워크



<figure><img src="../../.gitbook/assets/스크린샷 2024-02-20 오후 3.43.21.png" alt=""><figcaption></figcaption></figure>

포크/조인 프레임워크는 병렬화 할수 있는 작업을 재귀적으로 작은 작업으로 분할한 다음에 서브태스크 각각의 결과를 합쳐서 전체 결과를 만들도록 설계되었다.

포크/조인 프레임워크에서는 서브태스크를 스레드 풀의 작업자 스레드에 분산 할당하는 인터페이스를 구현한다.



## Recursive Task 활용

스레드 풀을 이용하려면 RecursiveTask 의 서브클래스를 만들어야 한다.

RecursiveTask 를 정의하려면 추상 메서드 compute를 구현해야 합니다.



자바에서 병렬 스트림 처리를 위한 포크/조인 프레임워크는 멀티코어 프로세서의 효율적인 활용을 돕는 자바 7의 핵심 기능 중 하나입니다. 포크/조인 프레임워크는 큰 작업을 작은 작업으로 분할하고, 각각의 작은 작업을 병렬로 실행한 다음 결과를 조합하는 방식으로 작동합니다. 이 프레임워크에서 `RecursiveTask`는 결과를 반환하는 작업을 구현할 때 사용됩니다.

`RecursiveTask`를 활용하는 방법은 다음과 같은 단계로 이루어집니다:

1. `RecursiveTask<V>`를 확장하는 클래스를 만듭니다. 여기서 `V`는 작업의 결과 타입입니다.
2. `compute` 메소드를 오버라이드하여 작업을 정의합니다. 만약 작업이 너무 크다면 작업을 더 작은 작업으로 분할하고 `fork` 메소드를 호출하여 분할된 작업을 비동기적으로 실행합니다.
3. 분할된 작업의 결과를 기다린 후 `join` 메소드를 호출하여 결과를 합칩니다.
4. 작업이 충분히 작다면 직접 계산을 수행합니다.
5. 모든 작업이 완료되면 최종 결과를 반환합니다.

아래는 `RecursiveTask`를 사용하는 간단한 예시입니다:

```java
import java.util.concurrent.RecursiveTask;

public class SumTask extends RecursiveTask<Long> {
    private final long[] numbers;
    private final int start;
    private final int end;
    public static final long THRESHOLD = 10_000;

    public SumTask(long[] numbers, int start, int end) {
        this.numbers = numbers;
        this.start = start;
        this.end = end;
    }

    @Override
    protected Long compute() {
        int length = end - start;
        if (length <= THRESHOLD) {
            // 직접 계산
            return add(numbers, start, end);
        } else {
            // 작업을 분할
            int middle = start + length / 2;
            SumTask task1 = new SumTask(numbers, start, middle);
            SumTask task2 = new SumTask(numbers, middle, end);
            
            // 비동기 실행
            task1.fork();
            task2.fork();
            
            // 결과 조합
            long task1Result = task1.join();
            long task2Result = task2.join();
            
            return task1Result + task2Result;
        }
    }

    private long add(long[] numbers, int start, int end) {
        long sum = 0;
        for (int i = start; i < end; i++) {
            sum += numbers[i];
        }
        return sum;
    }
}
```

위 예시에서는 배열의 합계를 계산하는 재귀적 작업을 정의합니다. 배열이 충분히 작으면 직접 합계를 계산하고, 그렇지 않으면 작업을 두 부분으로 분할하고 각 부분에 대해 새로운 `SumTask`를 생성하여 비동기적으로 실행합니다. 각 분할된 작업이 완료되면, 그 결과를 합쳐 최종 결과를 반환합니다.

이러한 방식으로 `RecursiveTask`를 사용하면 큰 데이터 세트를 효율적으로 병렬 처리할 수 있으며, 멀티코어 프로세서의 성능을 최대한 활용할 수 있습니다.



```java
import java.util.concurrent.ForkJoinPool;

public class SumTaskExample {
    public static void main(String[] args) {
        // 예시를 위한 큰 숫자 배열을 생성합니다.
        long[] numbers = new long[20000];
        for (int i = 0; i < numbers.length; i++) {
            numbers[i] = i;
        }

        // ForkJoinPool을 생성합니다.
        ForkJoinPool pool = new ForkJoinPool();

        // 최초의 SumTask를 생성하고, 전체 배열을 처리하도록 합니다.
        SumTask task = new SumTask(numbers, 0, numbers.length);

        // ForkJoinPool을 사용하여 작업을 실행하고 결과를 가져옵니다.
        long sum = pool.invoke(task);

        // 결과를 출력합니다.
        System.out.println("Sum: " + sum);
    }
}

```



