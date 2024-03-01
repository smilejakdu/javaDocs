# 성능 테스트 결과 해석하기

<figure><img src="../../.gitbook/assets/스크린샷 2024-03-01 오전 11.53.08.png" alt=""><figcaption></figcaption></figure>

요약 , 시나리오 카운트, codes , errors 가 나와있습니다.

<figure><img src="../../.gitbook/assets/스크린샷 2024-03-01 오후 12.03.39.png" alt=""><figcaption></figcaption></figure>

`artillery run --output report.json test-config.yaml`



```yaml
config:
  target: 'http://localhost:8080'
  phases:
    - duration: 30
      arrivalRate: 3
      name: Warm up
    - duration: 30
      arrivalRate: 3
      rampTo: 30
      name: Ramp up load
    - duration: 60
      arrivalRate: 30
      name: Sustained load
    - duration: 30
      arrivalRate: 30
      rampTo: 10
      name: End of load
scenarios:
  - name: "login and use some functions"
    flow:
      - post:
          url: "/login"
      - get:
          url: "/some-function-one"
      - get:
          url: "/some-function-two"
  - name: "just login"
    flow:
      - post:
          url: "/login"
```

이 YAML 파일은 성능 테스트를 위한 설정을 포함하고 있으며, 아마도 Artillery와 같은 부하 테스트 도구를 위한 것으로 보입니다. 여기에 정의된 내용을 바탕으로 각 부분의 의미와 목적을 설명하겠습니다.

#### Config 섹션:

* `target`: 이는 부하 테스트를 수행할 대상 서버의 URL입니다. 여기서는 `http://localhost:8080`으로 설정되어 있으므로, 로컬 서버가 테스트의 대상입니다.
* `phases`: 이 부분은 테스트의 다양한 단계를 정의합니다. 각 단계는 다음과 같습니다:
  * **Warm up**: 초기 30초 동안 사용자는 초당 3명씩 시스템에 도착합니다. 이 단계는 시스템을 '온난화'하는 데 사용됩니다.
  * **Ramp up load**: 다음 30초 동안 사용자 도착률이 초당 3명에서 30명으로 점차 증가합니다. 이는 시스템에 점점 더 많은 부하를 가하는 단계입니다.
  * **Sustained load**: 이후 60초 동안 사용자 도착률이 일정하게 초당 30명입니다. 이 단계는 시스템이 최대 부하를 지속적으로 처리할 수 있는지를 평가합니다.
  * **End of load**: 마지막 30초 동안 사용자 도착률이 초당 30명에서 10명으로 줄어듭니다. 이는 테스트의 종료 부분에서 시스템이 점차 덜 부하를 받게 되는 상황을 모방합니다.

#### Scenarios 섹션:

이 부분은 테스트 시 시뮬레이션할 사용자 시나리오를 정의합니다. 각 시나리오는 다음과 같습니다:

* **login and use some functions**: 이 시나리오에서 사용자는 로그인(`post` 요청) 후 두 가지 기능(`get` 요청)을 사용합니다. 이는 사용자가 로그인하고 일련의 작업을 수행하는 일반적인 사용 사례를 시뮬레이션합니다.
* **just login**: 이 시나리오에서 사용자는 단순히 로그인만 합니다. 이는 로그인 시스템에 대한 순수한 부하를 테스트하기 위한 시나리오입니다.

각 `post` 및 `get` 액션은 HTTP 요청을 나타냅니다. 예를 들어, `post` 요청은 `/login` 엔드포인트로 로그인을 시도하고, `get` 요청은 `/some-function-one` 및 `/some-function-two` 엔드포인트에 접근합니다.

이 설정 파일을 사용하여 성능 테스트를 실행하면, 정의된 시나리오와 부하 조건에 따라 서버에 요청을 보내고, 서버의 성능과 부하 처리 능력을 평가할 수 있습니다.



