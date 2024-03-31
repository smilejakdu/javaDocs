# 높은 부하를 받았을 때의 성능 테스트 결과 해석하기

<figure><img src="../../../.gitbook/assets/스크린샷 2024-03-01 오후 12.54.56.png" alt=""><figcaption></figcaption></figure>



한번요청할때마다 2천만번 반복하게 해서 부하가 걸리게 해서 테스트 해보도록 합니다.

```java
package com.example.stresstestbasic;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.ArrayList;
import java.util.concurrent.ThreadLocalRandom;

@RestController
public class HighLoadController {

    @RequestMapping(value = "/high-load-cpu", method = RequestMethod.GET)
    public int highLoadCpu() {
        int sum = 0;

        // 랜덤한 수를 뽑아서 더하는 연산
        for (int i = 0; i < 20_000_000; i++) {
            int randomInt = ThreadLocalRandom.current().nextInt();

            sum = sum + randomInt;
        }

        return sum;
    }

    @RequestMapping(value = "/high-load-memory", method = RequestMethod.GET)
    public int highLoadMemory() {
        ArrayList<Integer> memoryIntensiveList = new ArrayList<>();
        
        for (int i = 0; i < 500_000; i++) {
            // int를 추가하는 과정에서 새로운 Integer 래퍼 클래스 인스턴스 생성
            memoryIntensiveList.add(i);
        }

        return memoryIntensiveList.size();
    }
}

```

```yaml
config:
  target: 'http://localhost:8080'
  phases:
    - duration: 30
      arrivalRate: 20
      name: Warm up
    - duration: 10
      arrivalRate: 20
      rampTo: 200
      name: Ramp up load
    - duration: 10
      arrivalRate: 200
      name: Sustained load
    - duration: 30
      arrivalRate: 200
      rampTo: 20
      name: End of load
scenarios:
  - name: "high load cpu"
    flow:
      - get:
          url: "/high-load-cpu"
#  - name: "high load memory"
#    flow:
#      - get:
#          url: "/high-load-memory"
```



artillery run --output report.json high-load-test-config.yaml

입력을 해줍니다.



<figure><img src="../../../.gitbook/assets/스크린샷 2024-03-01 오후 3.04.16.png" alt=""><figcaption></figcaption></figure>

가설은 GC 가 많이 발생해서 생기는건가 ??? 가설을 세울수가 있다.

