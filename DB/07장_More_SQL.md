# Complex Queries

## WITH clause
- 특정 쿼리에서만 사용되고 사라지는 테이블.
- view와 비슷한 기능이지만 해당 SQL의 query 수행이 끝나면 정의부터 데이터까지 테이블이 완전히 사라진다.
- 예시: 아래 정의된 'BIGDEPTS' 테이블은 SQL 쿼리 내부에서만 존재한다.
    ```sql
    ---temporary table
    WITH BIGDEPTS (Dno) AS (
        select Dno
        from EMPLOYEE
        group by Dno
        having count(*) > 3 )
    ---temp table 끝
    select Dno, count(*)
    from employee
    where salary > 40000 and Dno in BIGDEPTS
    group by Dno;
    ```

    위에 먼저 정의되고 아래에서 사용된다. 

## Case construct
- 예시
    ```sql
    update employee
    set salary = 
    case when dno = 5 then salary + 2000
        when dno = 4 then salary + 1500
        else salary + 0;
    ```

## Recursive Queries in SQL (순환질의)
- specify a recursive query in a declarative manner
- with절 내에서 정의하는 sup_emp는 employee를, employee는 다시 sup_emp를 참조한다.
- 더이상 join 결과가 바뀌지 않을때 까지 join한다.
- 예시
    ```sql
    ---temp table 정의
    with RECURSIVE SUP_EMP (supssn, empssn) as (
        --- base query
        select super_ssn, ssn
        from employee
        union
        --- join할 임시 쿼리 SUP_EMP을 같이 구성한다.
        select S.supssn, E.ssn
        from employee as E, sup_emp as S
        where E.super_ssn = S.empssn )
    select *
    from sup_emp;
    ```

    EMPLOYEE | Super_ssn | Ssn | | SUP_EMP | Super_ssn | Ssn
    :---:|---|---|:---:|:---:|---|---
    - |e2 | e1 | - | - | e2 | e1
    - |e3 | e2 | - | - | e3 | e2
    - |`null` | e3 | - | - | `null` | e3


    위 두 테이블을 join한 결과. 튜플이 recursive하게 만들어진다.
    ```sql
    select *
    from employee as e, sup_emp as s
    where e.super_ssn = s.empssn
    ```
    SupSsn | EmpSsn | Super_ssn | Ssn
    --|--|--|--
    e3 | e2 | e2 | e1
    null | e3 | e3 | e2  

    최종적으로 출력되는 SUP_EMP
    SupSsn | EmpSsn
    ---|---
    e2 | e1
    e2 | e1
    `null` | e1
    e3 | e2
    `null` | e2
    `null` | e3

## Discussion and Summary of SQL Queries
- SELECT (att and func list)  
    FROM (table list)  
    WHERE (condition)  
    GROUP BY (group att(s))  
    HAVING (group contdition)  
    ORDER BY (att list)
- Query evaluation 순서  
    FROM(어느그룹?) - WHERE(튜플 필터링) - GROUP BY(남은 튜플 그루핑) - HAVING(조건에 맞는 그룹 선택) - ORDER BY(sort) 

### SQL Tuning Guide
- DBMS가 최대한 효율적으로 돌아가지만 실제론 쿼리에 따라 달라진다. 가이드를 따르자

# Specifying Constraints as Assertions and Actions as Triggers

## Create assertion
- 선언적인 assertion
    ```sql
    Create ASSERTION salary_constraint
    check(not exists (
        select *
        from employee E, employee M, department d
        where E.salary > M.Salary and
            E.dno = D.dnumber and D.mgr_ssn = m.ssn) )
    ```
- SQL을 수행한 결과가 `Where Clause`와 같으면 안된다. 쿼리 결과가 있으면 위반인 것이다.

## Assertion and CHECK Constraints
- Check Clause는 관련된 튜플이 업데이트 될때마다 CHECK해준다. 가능하면 이 CHECK를 쓰고 불가능시에만 Check Assertion을 이용한다.

## SQL Triggers
- Assertion 제약조건을 위배 했을 때의 대응