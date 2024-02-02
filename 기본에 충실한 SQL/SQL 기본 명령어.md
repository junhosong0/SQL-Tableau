# SQL 기본 명령어 분류
- DDL(Data Definition Language): 테이블 생성(Create), 변경(Alter, Rename), 삭제(Truncate, Drop)
- DML(Data Manipulation Language): 데이터 삽입(Insert), 조회(Select), 수정(Update), 삭제(Delete)
- DCL(Data Control Language): 특정 사용자에게 데이터 접근 권한 부여(Grant), 제거(Revoke)
- TCL(Transaction Control Languagae): DML 명렁어를 실행(Commit), 취소(Rollback), 임시 저장(Savepoint)


## 테이블 정의 (DDL)
테이블은 데이터를 보관하는 틀
- 행(row)과 열(column)로 이루어진 2차원 표임. Excel의 표를 생각하면 됨
- 테이블을 만들기 위해 DDL 명령어를 사용
- 각 열(column)마다 데이터 형식을 지정해줌. 반드시 1가지 데이터 형식만 저장
  - 데이터 형식: 문자, 숫자, 날짜, 논리형으로 분류되며 저장소 크기(바이트)에 따라 종류가 나뉨
  - 각 데이터 형식마다 저장소 크기를 지정하는 이유는 데이터 저장 공간 낭비를 줄이기 위해서
  - 테이블이나 컬럼을 다룰때는 백쿼트 ``를 사용하고 그 안에 들어가는 데이터를 다룰때는 따옴표 ''를 사용함

### 데이터베이스 생성(CREATE SCHEMA)
CREATE SCHEMA 'sql-tableau' ;
- 스키마 = 데이터베이스
- 스키마의 이름은 무조건 소문자로 지정됨

### 테이블 생성(CREATE TABLE)
```
-- [회원테이블] 생성
CREATE TABLE `회원테이블` (
  `회원번호` varchar(20) NOT NULL,
  `이름` varchar(20) DEFAULT NULL,
  `성별` varchar(2) DEFAULT NULL,
  `나이` int DEFAULT NULL,
  `가입금액` int DEFAULT NULL,
  `가입일자` date NOT NULL,
  `수신동의` bit(1) DEFAULT NULL,
  PRIMARY KEY (`회원번호`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```
- 책에서는 가입금액 컬럼의 데이터형식을 MS-SQL의 MONEY형식을 사용했으나 나는 MySQL을 쓰기 때문에 MONEY가 없어 INT로 대체했음
- MySQL에서는 PK 컬럼은 무조건 NOT NULL을 포함해줘야 함
- "--"는 주석 처리
- "```"(백쿼트)는 코드블록 삽입. 앞뒤로 넣어줌
- 테이블 확인: SELECT * FROM `sql-tableau`.회원테이블;

### 테이블 열 추가(ALTER TABLE ADD)
```
ALTER TABLE `회원테이블` ADD `몸무게` FLOAT(24)
```
- 열 추가시 반드시 데이터 형식 지정해주어야 함

### 테이블 열 이름 및 형식 변경 (ALTER TABLE CHANGE)
```
ALTER TABLE `회원테이블` CHANGE `몸무게` `몸무게(kg)` INT;
```
- ALTER TABLE 테이블명 CHANGE 기존컬럼명 변경할컬럼명 컬럼타입;
- 컬럼 타입만 변경할 경우: ALTER TABLE 테이블명 MODIFY 컬럼명 변경할컬럼타입;
- 컬럼 순서변경: ALTER TABLE 테이블명 MODIFY 순서변경할컬럼명 컬럼타입 AFTER 앞에오는 컬럼명;
- 컬럼 디폴트값 변경: ALTER TABLE 테이블명 ALTER COLUMN 변경할컬럼명 SET DEFAULT 디폴트값;
- 컬럼 삭제: ALTER TABLE 테이블명 DROP COLUMN 컬럼명;
- 테이블명 변경: ALTER TABLE 기존테이블명 RENAME 바꿀테이블명;

### 데이터 삭제(TRUNCATE) 및 테이블 삭제(DROP)
-- MEMBER 테이블 모든 행(전체) 데이터 삭제
```
TRUNCATE TABLE `MEMBER`;
```
-- MEMBER 테이블 자체 삭제
```
DROP TABLE MEMBER;
```


## 데이터 조작(DML)
DML(Data Manipulation Language)은 데이터를 삽입/조회/수정/삭제하기 위한 명령어

-- member 테이블 생성
```
CREATE TABLE `sql-tableau`.`member` (
  `회원번호` CHAR(20) NOT NULL,
  `이름` CHAR(20) NULL,
  `성별` VARCHAR(2) NULL,
  `나이` INT NULL,
  `가입금액` INT NULL,
  `가입일자` DATE NOT NULL,
  `수신동의` BIT,
  PRIMARY KEY (`회원번호`));
```

### 데이터 삽입(INSERT)
```
-- 데이터 선택
SELECT * FROM 'sql-tableau'.'member';

--데이터 입력
INSERT INTO `sql-tableau`.`member` (`회원번호`, `이름`, `성별`, `나이`, `가입금액`, `가입일자`, `수신동의`) VALUES ('A1001', '모원서', '남', 33, 100000, '2020-01-01', 1);
INSERT INTO `sql-tableau`.`member` (`회원번호`, `이름`, `성별`, `나이`, `가입금액`, `가입일자`, `수신동의`) VALUES ('A1002', '김영화', '여', 29, 200000, '2020-01-02', 0);
INSERT INTO `sql-tableau`.`member` (`회원번호`, `이름`, `성별`, `나이`, `가입금액`, `가입일자`, `수신동의`) VALUES ('A1003', '홍길동', '남', 29, 300000, '2020-01-03', NULL);
```

### 데이터 조회(SELECT)
-- 모든 열 조회
```
SELECT * FROM `MEMBER`;
```
- *은 테이블에 있는 모든 열을 조회 함
  
-- 특정 열 조회
```
SELECT `회원번호`,
`이름` AS `성명`,
`가입일자`,
`나이`
FROM MEMBER;
```
- MEMBER 테이블에 특정 컬럼명 조회 및 임시로 컬럼명 변경 (AS)

### 데이터 수정(UPDATE)
```
UPDATE `MEMBER` SET `나이` = 30;
```
- 모든 행을 조건 없이, 나이 30으로 수정하기

```
UPDATE MEMBER SET `나이` = 34 WHERE `회원번호` = 'A1001';
```
- 회원번호가 'A1001'인 데이터 행의 나이를 34로 변경하기
- WHERE는 조건필터 명령어

### 데이터 삭제(DELETE)
```
DELETE FROM MEMBER WHERE `회원번호` = 'A1001';
```
- 회원테이블의 회원번호가 A1001인 행데이터 삭제
- DELETE: 데이터만 삭제
- TRUNCATE: 데이터+테이블 공간 삭제
- DROP: 테이블 전체 삭제
  

  
  
