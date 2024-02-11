# CHAPTER 7 병렬 스트림



{% embed url="https://girawhale.tistory.com/131" %}

다른 언어와 달리 자바 Spring Boot 는 초기 설정시 쓰레드가 기본으로 20개 동작하게 설계가 되어있습니다.



**Spring Boot 2.5.4**에서 기본적으로 설정된 초기 스레드 풀 크기는 **20개**입니다.

하지만, 이 값은 다음과 같은 경우에 따라 달라질 수 있습니다.

* **Tomcat 버전:** Tomcat 버전에 따라 기본 설정값이 다를 수 있습니다.
* **Tomcat 설정:** `server.xml` 파일에서 `minSpareThreads` 속성을 설정하여 초기 스레드 풀 크기를 변경할 수 있습니다.
* **스프링 설정:** `spring.application.properties` 파일에서 `server.tomcat.min-threads` 속성을 설정하여 초기 스레드 풀 크기를 변경할 수 있습니다.

따라서, 정확한 초기 스레드 풀 크기를 알기 위해서는 다음 방법을 사용하는 것이 좋습니다.

* **Tomcat JMX 모니터링 도구:** 톰캣 JMX 모니터링 도구를 사용하여 현재 설정된 초기 스레드 풀 크기를 확인할 수 있습니다.
* **`server.xml` 파일 확인:** `server.xml` 파일에서 `minSpareThreads` 속성 값을 확인하여 초기 스레드 풀 크기를 확인할 수 있습니다.
* **`spring.application.properties` 파일 확인:** `spring.application.properties` 파일에서 `server.tomcat.min-threads` 속성 값을 확인하여 초기 스레드 풀 크기를 확인할 수 있습니다.



## Pyhon 에서의 쓰레드&#x20;

**싱글 스레드 환경에서 병렬 처리 가능 이유:**

* **GIL(Global Interpreter Lock):** Python 인터프리터는 GIL이라는 잠금 장치를 사용하여 한 번에 하나의 스레드만 실행하도록 제한합니다.
* **멀티프로세싱 모듈:** 멀티프로세싱 모듈은 여러 프로세스를 생성하여 GIL의 제약을 우회합니다. 각 프로세스는 독립적인 실행 컨텍스트를 가지므로 서로 동시에 작업을 처리할 수 있습니다.

Python의 멀티프로세싱 모듈은 싱글 스레드 환경에서도 병렬 처리를 가능하게 하는 강력한 도구입니다. 하지만 사용 시 고려해야 할 사항들이 있으므로, 작업의 특성에 따라 적절하게 사용하는 것이 중요합니다.



python 에서 싱글 쓰레드 환경에서 Threading 과 Multiprocessing 사용 가능한 이유는

Python은 싱글 쓰레드 인터프리터를 사용하여 한 번에 하나의 스레드만 실행합니다. 하지만 Threading과 Multiprocessing 모듈을 사용하여 병렬 처리를 구현할 수 있습니다.

**Threading:**

* **GIL(Global Interpreter Lock):** Python 인터프리터는 GIL이라는 잠금 장치를 사용하여 한 번에 하나의 스레드만 실행하도록 제한합니다.
* **Thread**는 GIL을 통해 순서대로 실행되므로 실제로 병렬 처리가 이루어지지는 않습니다.
* **하지만 I/O 작업**은 GIL에 의해 영향을 받지 않으므로 Threading을 사용하여 I/O 작업을 병렬 처리할 수 있습니다.
* **예시:** 웹 서버에서 여러 클라이언트 요청을 동시에 처리

**Multiprocessing:**

* **프로세스:** Multiprocessing은 여러 개의 프로세스를 생성하여 작업을 분산하여 병렬 처리합니다.
* **각 프로세스**는 독립적인 메모리 공간과 실행 컨텍스트를 가지므로 GIL의 영향을 받지 않습니다.
* **CPU 집중적인 작업**을 여러 프로세스에 분산하여 병렬 처리할 수 있습니다.
* **예시:** 데이터 처리, 과학 계산, 이미지 처리

**결론:**

* Python은 싱글 쓰레드 환경이지만 Threading과 Multiprocessing을 사용하여 I/O 작업과 CPU 집중적인 작업을 병렬 처리할 수 있습니다.
* 작업의 특성에 따라 적절한 방식을 선택하는 것이 중요합니다.

**참고자료:**

* Python 멀티 스레딩: [https://docs.python.org/3/library/threading.html](https://docs.python.org/3/library/threading.html): [https://docs.python.org/3/library/threading.html](https://docs.python.org/3/library/threading.html)
* Python 멀티프로세싱: [https://docs.python.org/3/library/multiprocessing.html](https://docs.python.org/3/library/multiprocessing.html): [https://docs.python.org/3/library/multiprocessing.html](https://docs.python.org/3/library/multiprocessing.html)



## Node 에서의 쓰레드

node js 에서 싱글 쓰레드 환경에서 병렬 처리 구현 방법은&#x20;

Node.js는 싱글 쓰레드 이벤트 루프 모델을 사용하여 비동기 I/O 작업을 효율적으로 처리하도록 설계되었습니다. 이는 Java의 병렬 스트림과는 다른 방식이지만, 비슷한 결과를 얻을 수 있습니다.

**Node.js에서 병렬 처리를 구현하는 방법:**

1. **비동기 I/O:** Node.js는 기본적으로 비동기 I/O를 지원합니다. 즉, I/O 작업이 완료될 때까지 기다리지 않고 다른 작업을 수행할 수 있습니다.
2. **콜백 함수:** 비동기 I/O 작업은 콜백 함수를 사용하여 완료 시점에 실행할 코드를 지정합니다.
3. **이벤트 루프:** Node.js는 이벤트 루프를 사용하여 콜백 함수를 실행하고 이벤트를 처리합니다.
4. **모듈 활용:** `async`, `libuv` 등 다양한 모듈을 사용하여 비동기 프로그래밍을 쉽게 구현할 수 있습니다.

**Node.js에서 병렬 처리 예시:**

* **웹 서버:** 여러 클라이언트 요청을 동시에 처리
* **데이터 처리:** 파일 읽기, 쓰기, 데이터 분석
* **네트워킹:** HTTP 요청, 웹 소켓

**Node.js와 Java 병렬 스트림 비교:**

| 특징       | Node.js             | Java 병렬 스트림 |
| -------- | ------------------- | ----------- |
| 실행 모델    | 싱글 쓰레드 이벤트 루프       | 멀티 스레드      |
| 병렬 처리 방식 | 비동기 I/O             | 멀티 스레드      |
| 장점       | 효율적인 메모리 사용, 간단한 구현 | 높은 성능       |
| 단점       | 코드 복잡성 증가, 디버깅 어려움  | 스레드 관리 필요   |

**결론:**

Node.js는 싱글 쓰레드 환경이지만 비동기 I/O와 이벤트 루프 모델을 사용하여 효율적인 병렬 처리를 구현할 수 있습니다. 작업의 특성에 따라 Node.js 또는 Java 병렬 스트림 중 적절한 방법을 선택해야 합니다.
