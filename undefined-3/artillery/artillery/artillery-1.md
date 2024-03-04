# Artillery 로 간단한 성능 테스트 진행해보기



{% embed url="https://github.com/lleellee0/stress-test-basic" %}



test-config.yaml 파일을 프로젝트 최상단 경로에 생성합니다.

```yaml
config:
  target: 'http://localhost:8080'
  phases:
    - duration: 10
      arrivalRate: 5
    - duration: 10
      arrivalRate: 20
    - duration: 30
      arrivalRate: 100
    - duration: 10
      arrivalRate: 20
scenarios:
  - flow:
    - get:
        url: "/hello"
```



`artillery run --output report.json test-config.yaml`

프로젝트를 실행시키고 나서 ,  위의 명령어를 입력하게 되면 성능 테스트가 진행이 된다.

<figure><img src="../../../.gitbook/assets/스크린샷 2024-03-01 오전 10.33.51.png" alt=""><figcaption></figcaption></figure>

10초마다 결과가 나오게 된다.

다 마치게 되면 report.json 파일이 생성이 됩니다.

&#x20;report.json 파일이 너무 길기때문에 보기가 어렵습니다. 그래서 html 파일로 변환을 해주도록 합니다.

<figure><img src="../../../.gitbook/assets/스크린샷 2024-03-01 오전 10.36.15.png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../../.gitbook/assets/스크린샷 2024-03-01 오전 10.36.42.png" alt=""><figcaption></figcaption></figure>



report.html 파일은 우리가 테스트 했던 결과 값을 html 로 볼 수 있습니다.



<figure><img src="../../../.gitbook/assets/스크린샷 2024-03-01 오전 10.37.46.png" alt=""><figcaption></figcaption></figure>

그러면 실행 결과가 나오게 됩니다.



