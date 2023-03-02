# MSA 분산된 db 환경에서의 트랜잭션 롤백처리에 대한 학습

- 기존 모놀로직한 구조에서는 분산 트랜잭션을 사용하여 오류가 발생했을 때 롤백을 시켜줄 수 있었다. 

# 하지만 MSA 의 여러 db 중 특정 db 에서 오류가 발생하여 롤백이 필요한 경우 어떻게 처리해야 할까?

## 1. 2PC (Two-Phase Commit)

- 2단계에 거쳐서 영속하는 작업을 말한다. 

가상의 서비스를 보며 이해해보자

<img alt="image" src="https://user-images.githubusercontent.com/73810834/222428219-43d6a69f-5484-482f-9f3d-fb0e4dea8670.png">

- 위의 과정 중 배당 서비스에서 오류가 발생한 경우 Transaction Coordinator 에서 각 서비스에서 Rollback이 필요하다는 메세지를 보낸다.

- NoSQL에서는 분산 트랜잭션을 지원하지 않으므로, Two Phase Commit의 경우 RDB에 한정적으로 사용할 수 있다. DB 트랜잭션을 사용하기 때문에 트랜잭션 교착상태가 발생할 수 있다는 단점이 있다.

- 추가로 Coordinator를 구성할 때 서로 다른 특징을 가진 DBMS를 사용하는 서비스 간에 구현이 쉽지 않다는 단점도 있다.


## 2. Saga

보상 트랜잭션 방식으로 자주 사용되는 방법이다. Saga는 약자가 아니며 Long Lived Transactions를 의미하는 단어이다.


Saga 패턴은 크게 두 가지로 구분할 수 있다.

- Choregraphy
- Ochestration

### Choregraphy-based Saga
- 순차적으로 이벤트가 전달되면서 트랜잭션이 관리되는 방식이다.

- 이벤트는 RabbitMQ, Kafka와 같은 메시지 큐 미들웨어를 사용해서 비동기 방식 혹은 분산 처리 형태로 전달할 수 있다.

<img alt="image" src="https://user-images.githubusercontent.com/73810834/222428988-29652387-93a0-49b1-8a27-cdf724fc59c2.png">

- 순차적으로 이벤트를 발행하면서 Commit을 한 후 중간에 에러가 터지면 취소 이벤트를 던지면서 각 서버의 데이터를 처리한다.

### Ochestration-based Saga
Saga Ochestrator에서 각 서비스를 호출하여 트랜잭션을 관리하는 방식이다.

<img alt="image" src="https://user-images.githubusercontent.com/73810834/222429190-5c4f3858-7eb3-4da7-b5f1-83d8a9f3f71a.png">

1. 주문이 처리되면 SAGA Ochestrator에게 트랜잭션을 시작할 것을 알린다.
2. Ochestrator는 결제 명령을 결제 서비스로 보내고 결제 완료 메시지를 응답받는다.
3. Ochestrator는 주문 준비 명령을 Order 서비스로 보내고 주문 준비 완료 메시지를 응답받는다.
4. Ochestrator는 배달 명령을 Delevery 서비스로 보내고 배달 처리 완료 메시지를 응답받는다.
5. 트랜잭션을 종료한다.

- 다음은 롤백 과정이다.

<img alt="image" src="https://user-images.githubusercontent.com/73810834/222429408-b50d3f98-1166-456a-8f69-62d09567e5bb.png">

1. 재고부족으로 인한 주문 취소 메시지를 Ochestrator에 전달한다.
2. Ochestrator에서는 보상 트랜잭션으로 환불 명령을 결제 서비스로 보내고 환불 완료 메시지를 응답받는다.
3. 트랜잭션을 종료한다.


Ochestration-based SAGA는 최종적으로 데이터 무결성을 보장하게 된다.

 
Ochestration-based SAGA는 트랜잭션을 Ochestrator가 집중 관리해주므로 복잡도가 줄어들고, 구현 및 테스트가 용이하다.


반면, Ochestrator 추가로 더 많은 인프라 자원을 사용해야 한다는 단점도 있다.


<추가>

모든 트랜잭션에 대해서 보상 트랜잭션을 만들어줄 필요는 없다. 읽기전용 혹은 항상 성공하는 단계의 트랜잭션의 경우는 보상 트랜잭션을 만들지 않아도 무방하다. (조회만 하는 경우를 말한다.)


## 다음을 수행해야 하는 경우 Saga 패턴을 사용합니다.

- 긴밀한 결합 없이 분산 시스템에서 데이터 일관성을 보장합니다.
- 시퀀스의 작업 중 하나가 실패하는 경우 롤백하거나 보상합니다.

## Saga 패턴은 다음에 덜 적합합니다.

- 강력하게 결합된 트랜잭션.
- 이전 참가자에서 발생하는 보상 트랜잭션.
- 순환 종속성.


# Reference

[jaehoney님 블로그](https://jaehoney.tistory.com/260)

[MicroSoft](https://learn.microsoft.com/ko-kr/azure/architecture/reference-architectures/saga/saga)