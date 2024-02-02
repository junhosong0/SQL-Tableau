# SQL 기본 명령어 분류
- DDL(Data Definition Language): 테이블 생성(Create), 변경(Alter, Rename), 삭제(Truncate, Drop)
- DML(Data Manipulation Language): 데이터 삽입(Insert), 조회(Select), 수정(Update), 삭제(Delete)
- DCL(Data Control Language): 특정 사용자에게 데이터 접근 권한 부여(Grant), 제거(Revoke)
- TCL(Transaction Control Languagae): DML 명렁어를 실행(Commit), 취소(Rollback), 임시 저장(Savepoint)

## 테이블 정의 (DDL)
테이블은 데이터를 보관하는 틀
- 행(row)과 열(column)로 이루어진 2차원 표임. Excel의 표를 생각하면 됨.
- 테이블을 만들기 위해 DDL 명령어를 사용.
- 각 열(column)마다 데이터 형식을 지정해줌. 반드시 1가지 데이터 형식만 저장.
  - 데이터 형식: 문자, 숫자, 날짜, 논리형으로 분류되며 저장소 크기(바이트)에 따라 종류가 나뉨
  - 각 데이터 형식마다 저장소 크기를 지정하는 이유는 데이터 저장 공간 낭비를 줄이기 위해서

### 데이터베이스 생성
CREATE SCHEMA 'sql-tableau' ;

- 스키마 = 데이터베이스
- 스키마의 이름은 무조건 소문자로 지정됨


### 테이블 생성
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

- 책에서는 가입금액 컬럼의 데이터형식을 MS-SQL의 MONEY형식을 사용했으나 나는 MySQL을 쓰기 때문에 MONEY가 없어 INT로 대체했음.
- MySQL에서는 PK 컬럼은 무조건 NOT NULL을 포함해줘야 함.
- "--"는 주석 처
