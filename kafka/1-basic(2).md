# 1. 카프카 기초 2

참고 강의: [인프런 - [데브원영] 아파치 카프카 for beginners](https://www.inflearn.com/course/%EC%95%84%ED%8C%8C%EC%B9%98-%EC%B9%B4%ED%94%84%EC%B9%B4-%EC%9E%85%EB%AC%B8/dashboard)

<br>

## 파티셔너 (Partitioner)

- 파티션을 효과적으로 사용하기 위함

<br>

### 원리

<img width="295" alt="image" src="https://github.com/minoflower31/TIL/assets/56334513/55749420-aeed-4111-8e7b-d8a488bb6d38">

- 프로듀서가 데이터를 보내면 무조건 파티셔너를 거쳐서 브로커에 전송됨
- 메시지 키 또는 메시지 값에 따라 파티션의 위치가 결정됨
- 파티셔너를 따로 설정하지 않으면, `UniformStickyPartitioner`로 설정됨

<br>

- 파티셔너 설정 O
    - 메시지 키는 해시값(파티션의 키)로 이루어짐
        - 동일한 키를 가진 메시지 키는 항상 동일한 파티션에 순서대로 들어가는 장점이 있음
- 파티셔너 설정 X
    - **배치** 단위로 파티셔너에 전달
    - **RR 방식**으로 적절히 분배됨
- 커스텀 파티셔너 제공
    - 파티셔너 인터페이스 존재
    - 어느 파티션에 데이터를 보낼 것인지 정할 수 있음
    - 언제 사용해?
        - VIP 고객을 위한 데이터 처리를 빠르게 하는 것

<br>
<br>

## Consumer Lag

- 프로듀서의 offset과 컨슈머의 offset 간의 차이
- lag은 여러 개가 존재할 수 있음
- offset: 프로듀서가 토픽의 파티션에 데이터를 보내게 되면 각 데이터에 붙는 숫자

<img width="334" alt="image" src="https://github.com/minoflower31/TIL/assets/56334513/a4660659-aa51-4208-a265-70b42e4e4b4e">

<br>

**Situation) 프로듀서가 데이터를 넣어주는 속도 > 컨슈머가 데이터를 가져가는 속도**

- offset의 차이가 발생 → **Consumer Lag**
- lag 숫자로 해당 토픽에 대한 프로듀서와 컨슈머의 상태 유추 가능
    - 주로 컨슈머 상태 확인 시 사용

Situation) 프로듀서 그룹이 1, Partition이 2개라면? Lag이 2개가 측정됨

- `records-lag-max`: 가장 높은 lag

<br>
<br>

## Consumer Lag 모니터링 - Burrow

https://github.com/linkedin/Burrow

**burrow**는 Lag을 모니터링하기 위한 오픈 소스이며, 독립적인 애플리케이션으로 실행가능

- (elastic search + grafana를 통해 확인할 수 있음)

<br>
하지만 컨슈머 단계에서 lag을 모니터링하면 위험하고 운영요소가 많이 들어감

- 컨슈머 로직 단에서 lag 수집 시 컨슈머 상태에 디펜던시가 걸리기 때문
- 컨슈머가 비정상적으로 종료 → lag 정보 보낼 수 없음 → 측정 불가
- 새로운 컨슈머마다 lag 정보를 특정 저장소에 저장하도록 로직 개발해야 함

<br>

### burrow 특징

1. 멀티 카프카 클러스터 지원
2. Sliding Window를 통한 컨슈머의 status 확인
    - status - error, warning, ok
        - error: 데이터가 계속 쌓이되, 컨슈머가 데이터를 가져가지 않을 시
        - warning: 데이터가 일시적으로 급격히 쌓여 offset이 증가
3. HTTP API 제공
    - 응답으로 애플리케이션에 저장해 둘 수 있음