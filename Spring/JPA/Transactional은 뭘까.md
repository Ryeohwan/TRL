# @Transactional 을 알아보자

- 데이터베이스의 상태를 변경하는 작업 또는 한번에 수행되어야 하는 연산들을 의미한다.
- begin, commit 을 자동으로 수행해준다.
- 예외 발생 시 rollback 처리를 자동으로 수행해준다.

## 트랜잭션은 4가지의 성질을 가지고 있다.

- 원자성(Atomicity)

    한 트랜잭션 내에서 실행한 작업들은 하나의 단위로 처리한다. 즉, 모두 성공 또는 모두 실패.
- 일관성(Consistency)

    트랜잭션은 일관성 있는 데이타베이스 상태를 유지한다. (data integrity 만족 등.)
- 격리성(Isolation)

    동시에 실행되는 트랜잭션들이 서로 영향을 미치지 않도록 격리해야한다.
- 영속성(Durability)

    트랜잭션을 성공적으로 마치면 결과가 항상 저장되어야 한다.

## 트랜잭션 처리 방법

    스프링에서는 간단하게 어노테이션 방식으로 @Transactional을 메소드, 클래스, 인터페이스 위에 추가하여 사용하는 방식이 일반적이다. 이 방식을 선언적 트랜잭션이라 부르며, 적용된 범위에서는 트랜잭션 기능이 포함된 프록시 객체가 생성되어 자동으로 commit 혹은 rollback을 진행해준다.


## @Transactional 옵션

1. isolation

    트랜잭션에서 일관성없는 데이터 허용 수준을 설정한다.

2. propagation

    트랜잭션 동작 도중 다른 트랜잭션을 호출할 때, 어떻게 할 것인지 지정하는 옵션이다

3. noRollbackFor

    특정 예외 발생 시 rollback하지 않는다.

4. rollbackFor

    특정 예외 발생 시 rollback한다.

5. timeout

    지정한 시간 내에 메소드 수행이 완료되지 않으면 rollback 한다. (-1일 경우 timeout을 사용하지 않는다)

6. readOnly

    트랜잭션을 읽기 전용으로 설정한다.


## Transactional 옵션 상세

1. isolation (격리레벨)

    @Transactional(isolation=Isolation.DEFAULT)
    public void addUser(UserDTO dto) throws Exception {
        // 로직 구현
    }

    - DEFAULT : 기본 격리 수준

        : 기본이며, DB의 lsolation Level을 따른다.

    - READ_UNCOMMITED (level 0) : 커밋되지 않는 데이터에 대한 읽기를 허용

        : 어떤 사용자가 A라는 데이터를 B라는 데이터로 변경하는 동안 다른 사용자는 B라는 아직 완료되지 않은(Uncommitted 혹은 Dirty)데이터 B를 읽을 수 있다.
        -Problem1 - Dirty Read 발생

    - READ_COMMITED (level 1) : 커밋된 데이터에 대해 읽기 허용

        : 어떠한 사용자가 A라는 데이터를 B라는 데이터로 변경하는 동안 다른 사용자는 해당 데이터에 접근할 수 없다.
        -Problem1 - Dirty Read 방지

    - REPEATEABLE_READ (level 2) : 동일 필드에 대해 다중 접근 시 모두 동일한 결과를 보장

        : 트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 shared lock이 걸리므로 다른 사용자는 그 영역에 해당되는 데이터에 대한 수정이 불가능하다.
        : 선행 트랜잭션이 읽은 데이터는 트랜잭션이 종료될 때까지 후행 트랜잭션이 갱신하거나 삭제가 불가능 하기 때문에 같은 데이터를 두 번 쿼리했을 때 일관성 있는 결과를 리턴한다.
        -Problem2 - Non-Repeatable Read 방지
    
    - SERIALIZABLE (level 3) : 가장 높은 격리, 성능 저하의 우려가 있음

        : 데이터의 일관성 및 동시성을 위해 MVCC(Multi Version Concurrency Control)을 사용하지 않음.
        (MVCC는 다중 사용자 데이터베이스 성능을 위한 기술로 데이터 조회 시 LOCK을 사용하지 않고 데이터의 버전을 관리해 데이터의 일관성 및 동시성을 높이는 기술)
        : 트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 shared lock이 걸리므로 다른 사용자는 그 영역에 해당되는 데이터에 대한 수정 및 입력이 불가능하다.
        -Problem3 - Phantom Read 방지

## 격리 수준에 따른 문제
|IsolationLevel|Dirty Read|Non-Repeatable Read|Phantom Read|
|---|---|---|---|
|Read Uncommitted|O|O|O|
|Read Committed|-|O|O|
|Repeatable Read|-|-|O|
|Serializable|-|-|-|

