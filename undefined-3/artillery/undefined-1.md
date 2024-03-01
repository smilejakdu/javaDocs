# 파라미터 활용하여 테스트하기



```yaml
config:
  target: 'http://localhost:8080'
  phases:
    - duration: 30
      arrivalRate: 3
      name: Warm up
  payload:
    path: "id-password.csv"
    fields:
      - "id"
      - "password"
scenarios:
  - name: "just login"
    flow:
      - post:
          url: "/login-with-id-password"
          json:
            id: "{{ id }}"
            password: "{{ password }}"
```

이후&#x20;

`artillery run --output report.json parameter-test-config.yaml`

이라고 입력하면 됩니다.&#x20;

테스트 스크립트를 실행시키면 테스트 진행이 됩니다.



Artillery로 HTTP 기반의 웹 애플리케이션에 대한 성능 테스트를 실행하는 데 사용됩니다. 각 구성 요소에 대해 자세히 설명하겠습니다:

#### Config 섹션:

* `target`: 이는 성능 테스트의 대상이 되는 서버의 기본 URL입니다. 여기서는 로컬 서버(`http://localhost:8080`)가 대상입니다.
* `phases`: 이는 테스트의 다양한 단계(또는 "페이즈")를 정의합니다. 이 예에서는 하나의 페이즈만 설정되어 있습니다:
  * **Warm up**: 초기 30초 동안 사용자는 초당 3명씩 시스템에 도착합니다. 이 단계는 시스템을 서서히 부하로 적응시키는 데 사용됩니다.
* `payload`: 이 부분은 테스트에 사용될 외부 데이터 파일과 해당 필드를 지정합니다.
  * `path`: 데이터 파일의 경로입니다. 여기서는 `"id-password.csv"`이며, 이 파일에는 로그인 테스트에 사용될 사용자 ID와 비밀번호가 포함되어 있어야 합니다.
  * `fields`: CSV 파일에서 사용할 필드의 이름입니다. 이 경우, `"id"`와 `"password"` 필드가 사용됩니다.

#### Scenarios 섹션:

이는 테스트 중에 시뮬레이션될 사용자 시나리오를 정의합니다. 이 예에서는 한 가지 시나리오만 설정되어 있습니다:

* **just login**: 이 시나리오는 사용자가 로그인하는 과정만을 시뮬레이션합니다.
  * `flow`: 사용자 시나리오의 흐름을 정의합니다. 여기서는 단일 `post` 요청이 포함됩니다:
    * `post`: 서버에 `POST` 요청을 보냅니다.
      * `url`: 요청을 보낼 URL입니다. 이 경우, `/login-with-id-password`로 설정되어 있습니다.
      * `json`: 요청 본문에 포함될 JSON 데이터입니다. `id`와 `password`는 CSV 파일에서 가져온 값으로 대체됩니다. `{{ id }}`와 `{{ password }}`는 Artillery가 페이로드 데이터에서 해당 값을 동적으로 교체하도록 지시하는 템플릿 변수입니다.

이 설정 파일을 사용하여 Artillery를 실행하면, 지정된 URL(`http://localhost:8080/login-with-id-password`)로 로그인 시도를 시뮬레이션하면서 성능 테스트를 수행합니다. CSV 파일에서 제공된 각 사용자 ID와 비밀번호 쌍을 사용하여, 설정된 기간 동안 초당 3회의 로그인 요청을 시스템에 보냅니다. 이를 통해 로그인 시스템의 성능과 부하 처리 능력을 평가할 수 있습니다.

<figure><img src="../../.gitbook/assets/스크린샷 2024-03-01 오후 12.34.25.png" alt=""><figcaption></figcaption></figure>

말고도 headers 에 값을 넣어서 테스트도 가능하며 weight 도 부여할 수 있습니다.





## Web Socket 테스트 하기



Artillery는 기본적으로 HTTP 및 HTTPS 요청을 테스트하기 위한 도구입니다. 하지만, Artillery는 WebSocket을 포함한 다양한 프로토콜을 지원하기 때문에 WebSocket 통신을 테스트하는 것도 가능합니다.

WebSocket 테스트를 위해서는 Artillery의 설정 파일을 WebSocket 프로토콜에 맞게 조정해야 합니다. WebSocket 통신을 테스트할 때는 연결을 열고, 메시지를 보내고, 서버로부터 메시지를 받는 것과 같은 작업을 시뮬레이션할 수 있습니다.

다음은 WebSocket 통신을 테스트하기 위한 Artillery 설정 파일의 예시입니다:

```yaml
config:
  target: "ws://localhost:8080"  # WebSocket 서버 주소
  phases:
    - duration: 60  # 테스트 지속 시간(초)
      arrivalRate: 10  # 초당 새 사용자 수
scenarios:
  - engine: "ws"  # WebSocket 프로토콜 사용
    flow:
      - send: "Hello, server!"  # 서버에 보낼 메시지
      - think: 1  # 다음 동작까지 대기 시간(초)
      - waitForResponse:  # 서버로부터 메시지를 기다림
      - send: "Another message"
      - think: 1
      - close  # WebSocket 연결 종료
```

위 설정 파일은 다음과 같은 작업을 시뮬레이션합니다:

1. WebSocket 서버(`ws://localhost:8080`)에 대한 연결을 시도합니다.
2. 연결이 성공하면, "Hello, server!" 메시지를 서버에 보냅니다.
3. 잠시 대기한 후(예: 1초), 서버로부터의 응답을 기다립니다.
4. 추가 메시지("Another message")를 보냅니다.
5. 다시 잠시 대기한 후 연결을 종료합니다.

WebSocket 테스트의 경우, `engine: "ws"` 설정이 필요하며, `flow` 섹션에서는 연결 설정, 메시지 전송, 응답 대기, 연결 종료 등의 동작을 정의할 수 있습니다. 이러한 설정을 통해 실제 웹 애플리케이션이나 서비스에서의 WebSocket 통신 패턴을 모델링하고 테스트할 수 있습니다.
