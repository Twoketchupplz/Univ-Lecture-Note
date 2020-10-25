# Update Operations
## CREATE DOMAIN
- `CREATE DOMAIN`
    ```sql
    create DOMAIN SSN_TYPE as CHAR(9);
    ```
- `PRIMARY KEY, FOREIGN KEY`
    ```sql
    create table employee (
        Fname   VARCHAR(15)     not null,
        Lname   CHAR,
        Primary KEY (PK 예시 Fname, ... 여러개 가능),
        Foreign KEY (Lname) References Employee(PK)
    );
    ```
- `UNIQUE`  
    - 같은 값이 불가능하게 한다.
    ```sql
    create table employee (
        -- Tuple 선언..
        Fname Varchar(15),
        -- pk, fk, ..
        Unique (Dname) --대체키가 된다.
    );
    ```
## Specifying Att Constraints and Att Defaults
### 참조 무결성 침해에 대응한다.
- `CONATRAINT`  
- `ON`  
    - `on delete` 삭제시
    - `on update` 갱신시
    constraint name을 주면 키 변경과 같은 수정이 가능하다.
    ```sql
    create ...(
    CONSTRAINT PK_constraint_name
        PRIMARY KEY (Ssn),
    CONSTRAINT constraint_name2
        FOREIGN KEY (Super_ssn) references emplyee (ssn)
            ON DELETE SET NULL ON UPDATE CASCADE
            --set default 도 가능
            -- 같은 줄에 on 여러번 가능하다.
    )

- `NOT NULL`
- `DEFAULT <value>`
    ```sql
    create table employee
        (--...
            Dno     int     not null    DEFAULT 1,
            --동시에 가능하다
        );
    ```

- `CHECK` clause
    - row가 추가되거나 수정되었을 때 체크한다.
    ```sql
    Dnumber INT not null
        CHECK (Dnumber > 0 and Dnuber < 21),
    CREATE DOMAIN D_num AS INTEGER
        CHECK (D_num > 0 and D_num < 21);
    ```
## DROP command
### 기존 구조물 삭제
- `DROP`
    ```sql
    DROP TABLE dependent RESTRICT;
    --or
    DROP SCHEMA company CASCADE;
    ```
    - opt: `Restrict, cascade`

## ALTER command
### 추가 제거
- `ADD`
- `DROP`
- `ALTER`
    ```sql
    ALTER TABLE SCHEMA_NAME.TABLE_NAME ADD Job VARCHAR(12);
    ALTER .. DROP COL_NAME CASCADE;
    ALTER .. ALTER COL_NAME DROP DEFAULT; -- att의 DEFAULT를 DROP
    ALTER .. ALTER .. SET DEFAULT;
    ALTER .. DROP CONSTRAINT CONST_NAME CASCADE; -- CONSTRAINT를 DROP하고 CONST_NAME을 ref하는건 CASCADE
    ```

## SELECT
### 기본형태
```sql
SELECT <att_list>  --검색대상
FROM <table_list>  --참조할 테이블
WHERE <condition>; --대상 조건
```
- Join query  
multi table로부터 정보를 검색하는 query.  
`WHERE` clause에서 att간 관계를 알려준다.
```sql
SELECT ..
FROM EMPLOYEE, DEPARTMENT
WHERE Dname='Research' AND Dnumber=Dno;
-- selection cond. AND join cond.
```

- `AS`  
Renaming  
att도 바꿀 수 있다.
```sql
SELECT ...
FROM EMPLOYEE AS E(Fn, Mi, ...);
```

- 모든 튜플에서 검색
```sql
SELECT  Ssn
FROM    EMPLOYEE; --모든 튜플에서 ssn 검색
```

- 검색 테이블 전체 att
```sql
SELECT *
FROM ...
```

- 중복된 결과를 허용여부
```sql
SELECT ALL att --default
--or--
SELECT DISTINCT att --중복제거

## set operations

# 데이터 타입
## Numeric
- INTEGER, SMALLINT
- FLOAT or REAL
- DOUBLE PRECISION
- DECIMAL(i, j) OR dec(i, j) or NUMERIC(i, j)
## Character string
- CHAR(n), VARCHAR(n), or CHAR VARYING(n)
- CLOB(Character Large OBject)
## Bit string
- BIT(n), BIT VARYING(n), BLOB(Binary Large OBject)
## Boolean
- B'10101'
## Date and Time
- DATE(YYYY-MM-DD), TIME(HH:MM:SS)
- TIMESTAMP(WITH TIME ZONE), INTERVAL