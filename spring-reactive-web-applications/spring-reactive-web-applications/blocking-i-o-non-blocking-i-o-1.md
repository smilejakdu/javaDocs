# Blocking I/O 방식과 Non-Blocking I/O 방식 이해하기 (1)



{% embed url="https://github.com/ITVillage-Kevin/spring-reactive-01" %}



<figure><img src="../../.gitbook/assets/스크린샷 2024-03-03 오후 7.56.33.png" alt=""><figcaption></figcaption></figure>



## Blocking I/O 방식 ( 요약 )

* 작업 쓰레드가 종료될때까지 요청을 한 쓰레드는 차단된다.
* 이러한 단점을 보완하기 위해 멀티 쓰레딩 기법을 통해 추가 쓰레드를 할당 할 수 있습니다.
*   CPU 대비 많은 수의 쓰레드를 사용하는 애플리케이션은 비효율적이다.

    * 컨텍스트 스위칭으로 인한 쓰레드 전환 비용 발생
    * 메모리 사용에 있어서 오버 헤드 발생
    * 쓰레드 풀어서의 응답 지연 문제 발생



<figure><img src="../../.gitbook/assets/스크린샷 2024-03-03 오후 8.12.12.png" alt=""><figcaption></figcaption></figure>

컨텍스트 스위치 할때 비용이 발생하는 이유가 뭘까요 ??



<figure><img src="../../.gitbook/assets/스크린샷 2024-03-03 오후 8.13.49.png" alt=""><figcaption></figcaption></figure>



## Blocking I/O 방식 ( 문제점 )



<figure><img src="../../.gitbook/assets/스크린샷 2024-03-03 오후 8.16.55.png" alt=""><figcaption></figcaption></figure>



## Non-Blocking I/O 방식

<figure><img src="../../.gitbook/assets/스크린샷 2024-03-03 오후 8.18.57.png" alt=""><figcaption></figcaption></figure>



