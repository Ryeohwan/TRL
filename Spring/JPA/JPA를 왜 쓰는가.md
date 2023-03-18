# JPA 를 쓰는 이유가 무엇일까?
    우리가 쓰는 관계형 db와 객체와의 간극이 있는데 이를 해소해주는 것이 JPA 이다.

## RDB 와 객체와의 차이
1. 상속
2. 연관 관계
3. 데이터 타입
4. 데이터 식별 방법

   
<br>

    기존 SQL 중심적인 개발에서 벗어나서 객체를 자바의 컬렉션에 저장하듯 db에 저장할 수 있게 하는 것이 JPA 이다.

# JPA 는 어플리케이션과 JDBC 사이에서 동작한다.

## JPA 소개
- jpa는 인터페이스의 모음

## JPA의 성능 최적화 기능

1. 1차 캐시와 동일성(identity) 보장
2. 트랜잭션을 지원하는 쓰기 지연(transactional write-behind)
3. 지연 로딩(Lazy Loading)

## 1차 캐시와 동일

1. 같은 트랜잭션 안에서는 같은 엔티티를 반환 - 약간의 조회 성능 향상
2. DB Isolation Level 이 Read Commit 이어도 애플리케이션에서 Repeatable Read 보장됨.