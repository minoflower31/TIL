# 0. 카프카 기초

참고 강의: [인프런 - [데브원영] 아파치 카프카 for beginners](https://www.inflearn.com/course/%EC%95%84%ED%8C%8C%EC%B9%98-%EC%B9%B4%ED%94%84%EC%B9%B4-%EC%9E%85%EB%AC%B8/dashboard)

<br>

# 카프카 개요 및 설명

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/156a3ada-709e-4dbe-84b4-90ec94334a73/Untitled.png)

Source Application과 TargetApplication이 증가함에 따라 데이터를 전송하기 까다로워짐

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0414fe3e-b5a0-463e-9c28-f3024305b8c4/Untitled.png)

- Kafka는 Queue와 같은 기능을 제공함. 큐에 넣는 원소는 Topic이라고 불림
- 데이터 손실 없이 복구 가능
- 낮은 지연율, 높은 처리량을 제공

# Topic

## 정의

- **데이터가 들어가는 공간을 의미**
- 카프카는 여러 가지의 Topic을 가질 수 있음
- Producer가 Topic에 데이터를 넣으면 Consumer가 Topic에서 데이터를 가져옴
- 목적에 따라 클릭 로그, send sms, location_log 와 같이 이름을 지정할 수 있음

## 내부 모습 및 동작

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fd045b4d-3a40-4b23-9d75-2f4930128116/Untitled.png)

- 끝에서부터 쌓이게 되는 구조
- Consumer는 가장 오래된 데이터(0)부터 가져옴
- 데이터를 가져온다 해서 이후에 삭제되지 않음
    - 새로운 Consumer가 해당 데이터를 다시 가져올 수 있는데 이는 조건이 존재함
        1. 컨슈머 그룹이 다를 경우
        2. auto.offset.reset = earliest 로 설정되어야 함
    

**New Situation - 파티션 1개가 늘어난 경우**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/63d3957d-26f3-4af6-a5cf-898ddd809eff/Untitled.png)

- 데이터를 넣을 때 Key를 지정할 수도 있다.
- [7] 데이터는 어디로 들어가야 할까?
    1. 키가 null이고 기본 파티셔너 사용 O → 라운드 로빈으로 할당
    2. 키가 있고, 기본 파티셔너 사용 O → 키의 해시값을 구한 후, 해당 파티션에 할당

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70a56e4a-c31a-47d0-bd38-6e7ae71e3e48/Untitled.png)

- 해당 예제에서는 키를 설정하지 않았다는 전제 하에 Round Robin으로 할당되는 모습을 확인할 수 있다.

**❗️주의) 파티션을 함부로 늘리면 안된다.**

- 파티션을 늘릴 순 있지만 줄일 수는 없기 때문
    - 잠깐, 파티션을 왜 늘릴까?
        - 파티션을 늘리면 둘 이상의 컨슈머가 존재할 경우 데이터 처리를 분산시킬 수 있기 때문
- 그럼 못 지워?
    - record 삭제는 설정값으로 이룰 수 있다.
        - 최대 record 보존 시간 `log.retention.ms`
        - 최대 record 보존 크기 (byte) `log.retention.byte`

# Broker, Replication, ISR

## Broker

- **카프카가 설치되어 있는 서버 단위**
- 보통 3개 이상의 브로커를 만들어 사용하는 것은 권장
- 예시 상황
    - 만약 파티션 1개, replication이 1인 Topic 존재, 브로커가 3대라면 브로커 3대 중 1대의 해당 토픽의 데이터가 저장됨

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/027bbc16-2b90-4c97-9f0c-57867004fa14/Untitled.png)

replication이 3이면 broker #3에서 사본 파티션이 생성됨

(broker count ≥ replication count)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8be9ff6c-354a-4e95-885e-58cdab65402b/Untitled.png)

원본은 Leader, 사본은 Follower라 불리는데 Leader, Follower 파티션을 합쳐서 **ISR**이라고 불림

## Replication

- 사용 이유: 파티션의 고가용성을 위해서 사용됨
- **ack**
    - Producer에 상세 옵션(0, 1, all)으로 존재
    - 파티션의 replication과 관련 있음
    - Option 0: Producer가 파티션에 데이터 전송 → Leader 파티션에 데이터가 정상적으로 전송됐는지 응답받지 않음
        - 속도는 빠르나 데이터 유실 가능성 존재
    - Option 1: Producer가 파티션에 데이터 전송 → Leader 파티션에 데이터가 정상적으로 전송됐는지 응답값 받음
        - 응답은 보장하나 나머지 파티션(=replication된 파티션)에 복제되었는지 알 수 없음
    - Option all: follwer 파티션들도 데이터가 저장되는 것을 확인하는 절차를 거침
        - 데이터 유실은 거의 존재하지 않으나 속도는 느림
        
- replication을 늘리는 것은 좋은가?
    - No. 브로커의 리소스 사용량이 늘어나기 때문에 비용 문제가 발생
    - `Tip` 카프카에 들어오는 데이터량, retention date를 고려하여 설계
    - 그래서 보통 replication을 3으로 설정하는 것이 Best.