# 데이터 베이스

## 데이터 베이스의 기본
- 데이터베이스: 일정한 규칙, 혹은 규약을 통해 구조화되어 저장되는 데이터의 모음
- 엔터티: 사람, 장소, 물건, 사건, 개념 등 여러 개의 속성을 지닌 명사
- 릴레이션: 데이터베이스에서 정보를 구분하여 저장하는 기본 단위, 릴레이션은 관계형 db 에서는 '테이블' 이라고 하며, NoSQL에서는 '컬렉션' 이라고 함.
- 속성: 릴레이션에서 관리하는 구체적이며 고유한 이름을 갖는 정보
- 도메인: 릴레이션에 포함된 각각의 속성들이 가질 수 있는 값의 집합

<br>

### 테이블과 컬렉션의 차이

<br>

MQSQL 구조는 

    레코드 - 테이블 - 데이터베이스로 이루어져 있음
    
MongoDB 데이터베이스의 구조는 

    도큐먼트 - 컬렉션 - 데이터베이스 로 이루어져 있음


## 키

- 기본키(Primary Key): 유일성과 최소성을 만족함.
- 자연키: 중복되는 것을 제외하고 자연스럽게 뽑아 결정하는 기본키, 언젠가 변함
- 인조키: MySQL의 auto increment 등 인조적으로 유일성을 확보하는 키. 기본키는 보통 인조키로 설정.
- 외래키: 다른 테이블의 기본키를 그대로 참조하는 값.
- 후보키: 기본키가 될 수 있는 후보들이며 유일성과 최소성을 동시에 만족하는 키
- 대체키: 후보키가 두 개 이상일 경우 어느 하나를 기본키로 지정하고 남은 후보키들
- 슈퍼키: 각 레코드를 유일하게 식별할 수 있는 유일성을 갖춘 키

<br>

## ERD(Entity Relation Diagram)의 기본

ERD는 데이터베이스를 구축할 때 가장 기초적인 뼈대 역할을 하며, 릴레이션 간의 관계들을 정의한 것. 

ERD는 시스템의 요구사항을 기반으로 작성되며 이 ERD를 기반으로 데이터베이스를 구축한다.

<br>

## 트랜잭션, 커밋, 롤백 그리고 트랜잭션 전파
- 트랜잭션: 데이터베이스에서 하나의 논리적 기능을 수행하기 위한 작업의 단위(퀴리를 묶는 단위)
- 커밋: 여러 쿼리가 성공적으로 처리되었다고 확정하는 명령어, 트랜잭션 단위로 수행되며 변경된 내용이 모두 영구적으로 저장.
- 롤백: 트랜잭션으로 처리한 하나의 묶음 과정을 일어나기 전으로 돌리는 일(취소)
- 트랜잭션 전파: 트랜잭션을 수행할 때 커넥션 단위로 수행하기 때문에 커넥션 객체를 넘겨서 수행해야 하는데 이를 넘겨서 수행하지 않고 여러 트랜잭션 관련 메서드의 호출을 하나의 트랜잭션에 묶이도록 하는 것이 트랜잭션 전파

## 트랜잭션의 특징 ACID
- 원자성: 'all or nothing'
    - 트랜잭션과 관련된 일이 모두 수행되었거나 되지 않았거나를 보장하는 특징
- 일관성
    - '허용된 방식' 으로만 데이터를 변경해야 하는 것을 의미함.
- 격리성
    - 트랜잭션 수행 시 서로 끼어들지 못하는 것을 말함.
- 지속성
    - 성공적으로 수행된 트랜잭션은 영원히 반영되어야 하는 것을 의미함.
- 무결성
    - 무결성이란 데이터의 정확성, 일관성, 유효성을 유지하는 것을 말함.

## 트랜잭션의 격리성
    트랜잭션은 무조건은 아니지만 서로 끼어들 수 없다.

- 이 격리성은 여러가지 단계가 있으며 해당 단계에 따라 격리성과 동시성의 정도가 다르다.
- 트랜잭션이 순차적으로 실행이 되면 격리성은 높아지지만 동시성은 너무 낮아져서 성능이 안좋게 된다.
- 격리성과 동시성은 반비례 관계이다!

![1](<https://user-images.githubusercontent.com/73810834/210349765-052749ea-b66e-45f6-93d6-ff872a05a847.png>)

