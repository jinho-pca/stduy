# DB_Intro

### 서울 5반 이민아

### 21.10.27

## Index

- 개념Primary Key 와 IndexDriving TableOptimizer (PREFIX / SUFIX)Query Plan (FULL TABLE SCAN/SINGLE ROW (CONSTANT))WHERE
- 공통 코드 테이블 코드
- 학생 테이블 코드

### 1. 개념

#### (1) Primary Key 와 Index

- Primary KeyKey, Value
- Index데이터베이스 테이블의 검색 속도를 향상시키기 위한 자료 구조pk가 없는 column에서 생성 가능
- Database OptimizerPREFIXSUFIX
- Query PlanFULL TABLE SCANSINGLE ROW (CONSTANT)

#### (2) Driving Table

- 테이블 조인 순서
- **A 테이블 10건,** B테이블 10000건인 경우 **A에서** B로 탐색

#### (3) Optimizer (PREFIX / SUFIX)

- Optimizer 강제 조절
- `SELECT /*+ JOIN_PREFIX(B) */` B테이블 **먼저 JOIN**
- *`SELECT /*+ JOIN_SUFIX(A) */` A테이블 **나중에 JOIN**

#### (4) Query Plan (FULL TABLE SCAN/SINGLE ROW (CONSTANT))

- 인덱스가 있는 경우CODE 테이블에 조건을 주지 않고 A.CODE_ID = B.CODE_ID 조인을 해서 모두 검색A테이블에 대한`AND A.CODE_ID = 'A'`의 조건이 없다면 모두 조인 후 A 테이블 모두 탐색 **FULL TABLE SCAN** + None Unique Key LookupA테이블에 대한 `AND A.CODE_ID = 'A'`의 조건이 추가되면 **SINGLE ROW (CONSTANT)** + None Unique Key Lookup
- 인덱스가 없는 경우**FULL TABLE SCAN + FULL TABLE SCAN**결과는 똑같이 산출되지만 인덱스가 없어서 두 테이블 모두 FULL TABLE SCANcode_id를 선택하고 Create Index 가능

#### (5) `WHERE 1 = 1`

- 편의성을 위해 추가한 WHERE절
- **항상 참인 조건 추가**하여 추후 주석 편하게 사용

### 2. 공통 코드 테이블 코드

#### (1) 카테고리 테이블 생성

- 서비스 내에는 분류 기준들이 존재
- 서비스 별로 많은 양의 기준 정보가 존재(각각 테이블로 관리 불가)
- 기준정보(코드) 모아 공통 코드 테이블에서 관리

```
CREATE TABLE `code_npk` (    `code_id` VARCHAR(5) NULL,    `code_name` VARCHAR(45) NULL,    `code_description` VARCHAR(200) NULL, ); CREATE TABLE `code_detail_npk` (    `code_detail_id` VARCHAR(5) NULL,    `code_id` VARCHAR(5) NULL,    `code_name` VARCHAR(45) NULL,    `code_description` VARCHAR(200) NULL, );
```

#### (2) 지역 정보 추가

```
INSERT INTO CODE_NPK (CODE_ID, CODE_NAME, DESCRIPTION) VALUES('A', '지역', 'SSAFY의 지역입니다.'); INSERT INTO CODE_DETAIL_NPK (CODE_DETAIL_ID, CODE_ID, CODE_NAME, DESCRIPTION) VALUES('A01', 'A', '서울', 'SSAFY의 서울 지역입니다.'); INSERT INTO CODE_DETAIL_NPK (CODE_DETAIL_ID, CODE_ID, CODE_NAME, DESCRIPTION) VALUES('A02', 'A', '대전', 'SSAFY의 대전 지역입니다.'); INSERT INTO CODE_DETAIL_NPK (CODE_DETAIL_ID, CODE_ID, CODE_NAME, DESCRIPTION) VALUES('A03', 'A', '구미', 'SSAFY의 구미 지역입니다.'); INSERT INTO CODE_DETAIL_NPK (CODE_DETAIL_ID, CODE_ID, CODE_NAME, DESCRIPTION) VALUES('A04', 'A', '광주', 'SSAFY의 광주 지역입니다.'); COMMIT;
```

#### (3) 지역 정보 조회

- 인덱스가 있는 경우CODE 테이블에 조건을 주지 않고 A.CODE_ID = B.CODE_ID 조인을 해서 모두 검색`AND A.CODE_ID = 'A'`의 조건이 없다면 모두 조인 후 A 테이블 모두 탐색 **FULL TABLE SCAN** + None Unique Key Lookup`AND A.CODE_ID = 'A'`의 조건이 추가되면 **SINGLE ROW (CONSTANT)** + None Unique Key Lookup

```
SELECT * FROM CODE_DETAIL; SELECT *  FROM CODE A, CODE_DETAIL B WHERE 1 = 1 -- 편의성을 위해 추가한 WHERE절 (항상 참인 조건 추가하여 주석으로 편하게 사용) AND A.CODE_ID = 'A' -- FULL TABLE SCAN에서 SINGLE ROW (CONSTANT)로 변경 AND A.CODE_ID = B.CODE_ID
```

- 인덱스가 없는 경우**FULL TABLE SCAN + FULL TABLE SCAN**결과는 똑같이 산출되지만 인덱스가 없어서 두 테이블 모두 FULL TABLE SCANcode_id를 선택하고 Create Index 가능

```
SELECT * FROM CODE_DETAIL; SELECT *  FROM CODE_NPK A, CODE_DETAIL_NPK B WHERE 1 = 1 -- 편의성을 위해 추가한 WHERE절 (항상 참인 조건 추가하여 주석으로 편하게 사용) AND A.CODE_ID = 'A' AND A.CODE_ID = B.CODE_ID
```

#### (4) 기수 정보 추가

```
INSERT INTO CODE_NPK (CODE_ID, CODE_NAME, DESCRIPTION) VALUES('B', '기수', 'SSAFY의 기수입니다.'); INSERT INTO CODE_DETAIL_NPK (CODE_DETAIL_ID, CODE_ID, CODE_NAME, DESCRIPTION) VALUES('A06', 'B', '6기', 'SSAFY의 기수는 6기입니다.'); COMMIT;
```

#### (5) 기수 정보 조회

- **FULL TABLE SCAN + FULL TABLE SCAN**

```
SELECT * FROM CODE A, CODE_DETAIL B WHERE 1 = 1;
```

- 모두 조인 후 A 테이블 모두 탐색 **FULL TABLE SCAN** + None Unique Key Lookup

```
SELECT * FROM CODE A, CODE_DETAIL B WHERE 1 = 1 AND A.CODE_ID = B.CODE_ID;
```

- HINT (Optimizer 강제 조절)`SELECT /*+ JOIN_PREFIX(B) */` B테이블 먼저 JOIN`SELECT /*+ JOIN_SUFFIX(A)` A테이블 나중에 JOIN

```
SELECT /*+ JOIN_PREFIX(B) */ * # SELECT /*+ JOIN_SUFFIX(A) */ * FROM CODE A, CODE_DETAIL B WHERE 1 = 1 AND A.CODE_ID = B.CODE_ID;
```

### 3. 학생 테이블 코드

#### (1) 학생 테이블 추가

```
CREATE TABLE `STUDENT` (    `STUDENT_id` VARCHAR(5) NULL,    `STUDENT_NAME` VARCHAR(20) NULL,    `STUDENT_AREA` VARCHAR(45) NULL,    `STUDENT_NUMBER` VARCHAR(200) NULL,    PRIMARY KEY (`code_id`, `code_detail_id`));
```

#### (2) 학생 정보 추가

```
INSERT INTO STUDENT (STUDENT_ID, STUDENT_NAME, STUDENT_AREA, STUDENT_NUMBER)VALUES('STU01', '이민아', 'A01', 'B05');COMMIT;
```

#### (3) 학생 정보 전체 조회 JOIN

- **FULL TABLE SCAN + FULL TABLE SCAN + FULL TABLE SCAN**

```
# 전체 조회 JOINSELECT A.STUDENT_ID, A.STUDENT_NAME, B.CODE_NAME, C.CODE_NAMEFROM STUDENT A, CODE B, CODE_DETAIL CWHERE A.SSAFY_AREA = B.CODE_DETAIL_IDAND A.SSAFY_NUMBER = C.CODE_DETAIL_ID
```

#### (3) 학생 정보 특정 조회 JOIN

- **SINGLE ROW (CONSTANT)** **+** **SINGLE ROW (CONSTANT) + SINGLE ROW (CONSTANT)**

```
# 특정 조회 JOINSELECT A.STUDENT_ID, A.STUDENT_NAME, B.CODE_NAME, C.CODE_NAMEFROM STUDENT A, CODE B, CODE_DETAIL CWHERE 1 = 1-- 편의성을 위해 추가한 WHERE절 (항상 참인 조건 추가하여 주석으로 편하게 사용)AND A.STUDENT_ID = 'STU01'-- JOIN할 때 조건 필수1AND A.SSAFY_AREA = B.CODE_DETAIL_IDAND B.CODE_ID = 'A'-- JOIN할 때 조건 필수2AND A.SSAFY_NUMBER = C.CODE_DETAIL_IDAND C.CODE_ID = 'B'-- JOIN할 때 조건 필수3
```

#### (4) 학생 정보 특정 조회 SUB QUERY

- **SINGLE ROW (CONSTANT)** **+** **SINGLE ROW (CONSTANT) + SINGLE ROW (CONSTANT)**

```
# SUB QUERYSELECT         A.STUDENT_ID,         A.STUDENT_NAME,        (            SELECT                CODE_NAME            FROM CODE_DETAIL CD            WHERE A.SSAFY_AREA = CD.CODE_DETAIL_ID            AND CD.CODE_ID = 'A'        ) SSAFY_AREA,        (            SELECT                CODE_NAME            FROM CODE_DETAIL CD            WHERE A.SSAFY_NUMBER = CD.CODE_DETAIL_ID            AND CD.CODE_ID = 'B'        ) SSAFY_NUMBERFROM STUDENT A-- JOIN TABLE 2개는 제거WHERE 1 = 1-- 편의성을 위해 추가한 WHERE절 (항상 참인 조건 추가하여 주석으로 편하게 사용)AND A.STUDENT_ID = 'STU01'
```