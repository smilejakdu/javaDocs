# 수업 사전 준비



## 수업 준비 혹은 공부 순서&#x20;

<figure><img src="../.gitbook/assets/스크린샷 2024-03-03 오후 9.34.49.png" alt=""><figcaption></figcaption></figure>

Spring Boot WebFlux 를 학습하게 된다면 위의 공부 순서대로 공부하면 될것 같습니다.



## 실습 환경 구축



<figure><img src="../.gitbook/assets/스크린샷 2024-03-03 오후 9.46.29.png" alt=""><figcaption></figcaption></figure>



우선 프로젝트를 생성합니다.

### Dependencis 설정

```gradle
dependencis {
    implementation 'org.springframework.boot:spring-boot-starter-webflux'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testImplementation 'io.projectreactor:reactor-test'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
}
```

<figure><img src="../.gitbook/assets/스크린샷 2024-03-03 오후 9.58.50.png" alt=""><figcaption></figcaption></figure>

{% embed url="https://github.com/ITVillage-Kevin/spring-reactive-01" %}



