# Relational Data Model

## Relational Model Concepts
- proposed by Codd of IBM
- 관계로 표현한 데이터베이스

## Informal Definations  
- Relation, Table
    - A table of values 값들의 표.
    - 테이블 내에 내용을 relation state라고 한
    
- Row, Tuple
    - 각 행의 관계는 identifier가 주어질 수 있다.
    - Extention: 실제 있는 값

- Identifier(식별자)
    - 모든 행이 다른값을 가지고 있는 것.

- *Domain*, *Attribute*, *Column*
    - Be called by its col name or col header or att name  
    - D is a set of atomic(indivisible) values. 
    - A domain is given a name, data type, and format
    - Col 들어갈 수 있는 값의 범위, 속성을 정의하고 있다.
    - 도메인이 갖는 속성의 정의가 되고 속성이 가질 수 있는 데이터 타입 포멧을 가진다.

- *Schema*
    - Relation schema, R(A1, A2, ... , An)
    - Intension(of relation)  
    - relation의 차수(or arity)는 그 스키마의 att의 개수이다.  
    e.g. STUDENT(Name:string, Ssn:String)

## Characteristics of Relations
- Relational Model 튜플은 순서가 없다.




# Relational DB Constraints

## Referential Intergirty (참조무결성)
- 튜플 간 관계를 특정하기 위함
