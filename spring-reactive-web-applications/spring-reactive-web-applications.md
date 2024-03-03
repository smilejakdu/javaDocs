# Spring Reactive Web Applications 개요



{% embed url="https://en.wikipedia.org/wiki/Reactive_programming" %}

리액티브 프로그래밍의 특징

* 데이터 소스에 변경이 있을때마다 데이터를 전파
* 선언형 프로그래밍 패러다임: 실행할 동작을 구체적으로 명시하지 않고 목표만 정의함
* 함수형 프로그래밍 기법 사용



## 명령형 프로그래밍 vs 선언형 프로그래밍

<figure><img src="../.gitbook/assets/스크린샷 2024-03-03 오후 7.48.25.png" alt=""><figcaption></figcaption></figure>

Reactive Streams 를 구현한 구현체

* RxJava
* Java 9 Flow API
* Akka Streams
* Reactor
* 그외 RxJS, RxScala, RxAndroid 등의 리액티브 확장



위의 종류중에서 우리는 리액터에 대해서 학습을 진행할 예정입니다.

