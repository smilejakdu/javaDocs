# CHAPTER 6 스트림으로 데이터 수집



<figure><img src="../.gitbook/assets/스크린샷 2024-02-11 오후 7.42.14.png" alt=""><figcaption></figcaption></figure>

위와 같은 로직을 stream 을 사용했을때 , 변경이 가능하다.



<figure><img src="../.gitbook/assets/스크린샷 2024-02-11 오후 7.42.54.png" alt=""><figcaption></figcaption></figure>



## 컬렉터란 무엇인가 ?

스트림의 요소를 어떤 식으로 도출할지 지정한다.

그래서 map 과 filter 등을 적용하고 난뒤에 마지막에 어떤식으로 결과값을 도출할지 컬렉터로 지정한다.



<figure><img src="../.gitbook/assets/스크린샷 2024-02-11 오후 8.09.56.png" alt=""><figcaption></figcaption></figure>

```java
List<Transaction> transactions = 
    transactionStream.collect(Collectors.toList());
```



## 최대값 최소값 구하기

<figure><img src="../.gitbook/assets/스크린샷 2024-02-11 오후 8.16.36.png" alt=""><figcaption></figcaption></figure>



## 요약 연산

<figure><img src="../.gitbook/assets/스크린샷 2024-02-11 오후 8.20.46.png" alt=""><figcaption></figcaption></figure>

```java
double avgCalories = 
    menu.stream().collect(averagingInt(Dish::getCalories));
    
IntSummaryStatistics menuStatistics =
    menu.stream().collect(summarizingInt(Dish::getCalories));
```







