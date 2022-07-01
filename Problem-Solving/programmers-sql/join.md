## 없어진 기록 찾기

https://programmers.co.kr/learn/courses/30/lessons/59042

> Q. 입양을 간 기록은 있는데, 보호소에 들어온 기록이 없는 동물의 ID와 이름을 ID 순으로 조회하는 SQL문을 작성해주세요.

<br>

```mysql
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_OUTS as OUTS
LEFT OUTER JOIN ANIMAL_INS as INS ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE INS.ANIMAL_ID is null
```
<br>

### 문제에 쓰인 문법
+ `FROM` 테이블명 `left outer join` 조인할 [테]()이블명 `ON` 테이블명.id = 조인할 테이블명.id
+ `IS NULL`: null인 컬럼만 조회하겠다

<br>


## 있었는데요 없었습니다

https://programmers.co.kr/learn/courses/30/lessons/59043

> Q. 보호 시작일보다 입양일이 더 빠른 동물의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 시작일이 빠른 순으로 조회해야합니다.

<br>

```mysql
SELECT INS.ANIMAL_ID, INS.NAME
FROM ANIMAL_INS as INS
LEFT OUTER JOIN ANIMAL_OUTS as OUTS ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE INS.DATETIME > OUTS.DATETIME
ORDER BY INS.DATETIME
```
<br>

### 문제에 쓰인 문법
+ `FROM` 테이블명 `left outer join` 조인할 테이블명 `ON` 테이블명.id = 조인할 테이블명.id
+ `>`: 대소 크기 비교


<br>


## 오랜 기간 보호한 동물(1)

https://programmers.co.kr/learn/courses/30/lessons/59044

> Q. 아직 입양을 못 간 동물 중, 가장 오래 보호소에 있었던 동물 3마리의 이름과 보호 시작일을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 시작일 순으로 조회해야 합니다.

<br>

```mysql
SELECT INS.NAME, INS.DATETIME
FROM ANIMAL_INS AS INS
         LEFT JOIN ANIMAL_OUTS AS OUTS ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE OUTS.ANIMAL_ID is null
ORDER BY INS.DATETIME
LIMIT 3
```
<br>

### 문제에 쓰인 문법
+ `FROM` 테이블명 `left outer join` 조인할 테이블명 `ON` 테이블명.id = 조인할 테이블명.id
+ `LIMIT n`: 상위 n개만 조회하겠다

<br>

## 보호소에서 중성화한 동물

https://programmers.co.kr/learn/courses/30/lessons/59045

> Q. 아직 입양을 못 간 동물 중, 가장 오래 보호소에 있었던 동물 3마리의 이름과 보호 시작일을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 시작일 순으로 조회해야 합니다.

<br>

```mysql
SELECT INS.ANIMAL_ID, INS.ANIMAL_TYPE, INS.NAME
FROM ANIMAL_INS as INS
         LEFT JOIN ANIMAL_OUTS as OUTS ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE SEX_UPON_INTAKE LIKE 'Intact%'
  AND (SEX_UPON_OUTCOME LIKE 'Spayed%' || SEX_UPON_OUTCOME LIKE 'Neutered%')
ORDER BY INS.ANIMAL_ID
```

또는

```mysql
SELECT INS.ANIMAL_ID, INS.ANIMAL_TYPE, INS.NAME
FROM ANIMAL_INS as INS
LEFT JOIN ANIMAL_OUTS as OUTS ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE SEX_UPON_INTAKE != SEX_UPON_OUTCOME
ORDER BY INS.ANIMAL_ID
```
<br>

### 문제에 쓰인 문법
+ `LIKE`
  + `LIKE 'A%'`: A로 시작하는 문자열 찾기
  + `LIKE '%A'`: A로 끝나는 문자열 찾기
  + `LIKE '%A%'`: A를 포함한 문자열 찾기
