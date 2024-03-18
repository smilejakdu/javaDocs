# Spring MVC vs Spring WebFlux







<figure><img src="../../.gitbook/assets/스크린샷 2024-03-03 오후 8.21.40.png" alt=""><figcaption></figcaption></figure>

Spring Boot MVC와 Spring Boot WebFlux는 Spring Framework에서 웹 애플리케이션을 구축하기 위한 두 가지 다른 접근 방식입니다. 각각의 차이점과 장단점을 아래에 설명합니다.

#### Spring Boot MVC

Spring Boot MVC는 Spring의 전통적인 서블릿 기반 웹 프레임워크입니다. Spring MVC(Model-View-Controller)는 Spring Framework의 일부로, 동기식 처리와 블로킹 I/O를 사용하여 웹 애플리케이션을 개발합니다.

**장점:**

1. **성숙도와 안정성:** Spring MVC는 오랜 시간 동안 사용되어 왔으며, 광범위한 사용 사례와 문서가 있어 안정적이고 성숙한 기술입니다.
2. **동기식 프로그래밍 모델:** 많은 개발자들이 익숙해져 있는 전통적인 동기식 프로그래밍 모델을 사용합니다.
3. **광범위한 커뮤니티 지원:** Spring MVC는 광범위한 커뮤니티 지원과 리소스를 가지고 있어 문제 해결이 용이합니다.

**단점:**

1. **스레드 효율성:** 각 요청은 별도의 스레드에서 처리되므로 동시에 많은 요청을 처리할 때 스레드 리소스가 부족할 수 있습니다.
2. **스케일링 제한:** 높은 트래픽 시나리오에서는 스레드 기반 모델이 한계에 도달할 수 있어 확장성이 제한될 수 있습니다.

#### Spring Boot WebFlux

Spring Boot WebFlux는 Spring 5에 도입된 새로운 반응형 웹 프레임워크입니다. WebFlux는 비동기식, 논블로킹 I/O를 사용하며, 반응형 스트림을 기반으로 동작합니다.

**장점:**

1. **비동기식 및 논블로킹:** 비동기식 프로그래밍 모델을 사용하여 높은 동시성과 스케일링을 가능하게 합니다.
2. **반응형 스트림 지원:** 데이터 스트림을 효율적으로 처리하고 백프레셔(backpressure)를 관리할 수 있습니다.
3. **리소스 효율성:** 더 적은 수의 스레드와 리소스를 사용하여 동시에 많은 요청을 처리할 수 있습니다.

**단점:**

1. **학습 곡선:** 비동기식 프로그래밍 모델과 반응형 프로그래밍 개념은 일부 개발자에게 어려울 수 있으며, 학습 곡선이 높습니다.
2. **라이브러리와의 호환성:** 모든 타사 라이브러리가 비동기식 또는 논블로킹 방식으로 작동하는 것은 아니므로 호환성 문제가 발생할 수 있습니다.
3. **새로운 기술:** 비교적 새로운 기술이기 때문에 Spring MVC만큼 광범위한 커뮤니티 지원이나 문서를 가지고 있지 않을 수 있습니다.

결론적으로, Spring Boot MVC와 WebFlux 선택은 애플리케이션의 요구 사항, 트래픽 예상, 개발 팀의 기술 수준 등 다양한 요소를 고
