# 프로듀서 예제



<figure><img src="../.gitbook/assets/스크린샷 2024-03-30 오후 9.27.21.png" alt=""><figcaption></figcaption></figure>

프로듀서의 전송 방법은 메시지를 보내고 확인하지 않기, 동기 전송, 비동기 전송 이라는 세 가지 방식으로 크게 나눌 수 있습니다. 이제부터 각 전송 방법에 따른 예제 코드와 특징을 차례대로 살펴보겠습니다.&#x20;



```java
public class PeterProducerCallback implements Callback {
    private ProducerRecord<String, String> record;
    
    public PeterProducerCallback(ProducerRecord<String, String> record) {
        this.record = record;
    }
    
    @Override
    public void onCompletion(RecordMetadata metadata, Exception e) {
        if (e != null) {
            e.printStackTrace();
        } else {
            System.out.printf("Topic: %s, Partition: %d, Offset: %d, Key: %s,
            Received Message: $s\n", metadata.topic(), metadata.partition(), metadata.offset(),
            record.key(), record.value());
        }
    }
}
```



1. 콜백을 사용하기 위해 org.apache.kafka.clients.producer.Callback 을 구현하는 클래스가 필요함
2. 카프카가 오류를 리턴하면 onCompletion() 은 예외를 갖게 되며 , 실제 운영 환경에서는 추가적인 예외 처리가 필요함



이제는 비동기 프로듀서에 대해서 알아보도록 합니다.&#x20;



## 비동기 프로듀서

비동기 방식으로 전송하면 빠른 전송이 가능합니다.

컨슈머는 카프카의 토픽에 저장되어 있는 메시지를 가져오는 역할을 담당합니다.

