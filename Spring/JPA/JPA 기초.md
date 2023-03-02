# Spring 기초

# 스프링 빈이란?
    스프링 IoC 컨테이너가 관리하는 자바 객체를 말한다.

- 스프링에서 POJO(Plain Old Java Object)를 beans라고 부른다.
    - 간단하게 getter / setter를 생각하면 될 것 같다.
- Spring Framework에서는 ApplicationContext.getBean()과 같은 메서드를 이용하여 직접 호출할 수 있다.

# IoC 컨테이너는 뭔데
    IoC (Inversion of Control)

인스턴스 생성부터 소멸까지의 인스턴스 생명주기 관리를 개발자가 하는 것이 아니라 컨테이너가 대신 해주는 것을 제어의 반전이라고 한다.

    스프링 프레임워크도 객체를 생성하고 관리하고 책임지고 의존성을 관리해주는 컨테이너가 있는데, 그것이 바로 IoC 컨테이너(= 스프링 컨테이너)다.

## 하는 일은?
- IoC 컨테이너는 객체의 생성을 책임지고, 의존성을 관리한다.
- POJO의 생성, 초기화, 서비스, 소멸에 대한 권한을 가진다.
- 개발자들이 직접 POJO를 생성할 수 있지만 컨테이너에게 맡긴다.
- 개발자는 비즈니스 로직에 집중할 수 있다.
- 객체 생성 코드가 없으므로 TDD가 용이하다. 
    - TDD란 Test Driven Development의 약자로 ‘테스트 주도 개발’이라고 한다.
    - 반복 테스트를 이용한 소프트웨어 방법론으로 작은 단위의 테스트 케이스를 작성하고 이를 통과하는 코드를 추가하는 단계를 반복하여 구현한다.

## 분류
DL(Dependency Lookup) 과 DI (Dependency Injection)
- DL : 저장소에 저장되어 있는 Bean에 접근하기 위해 컨테이너가 제공하는 API를 이용하여 Bean을 Lockup하는 것
- DI : 각 클래스간의 의존관계를 빈 설정(Bean Definition) 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것
    - Setter Injection (수정자 주입)
    - Constructor Injection (생성자 주입)
    - Method Injection (필드 주입)

<br>

    DL 사용시 컨테이너 종속이 증가하기 때문에 주로 DI를 사용한다.

## DI(Dependency Injection) 알아보기
스프링 빈을 등록하는 두 가지 방법 (@Component, @Bean)

<br>

## 스프링 컨테이너 (= IoC 컨테이너)의 종류
스프링 컨테이너가 관리하는 객체를 빈(Bean)이라고 하고,

이 빈들을 관리한다는 의미로 컨테이너를 빈 팩토리(BeanFactory) 라고 부른다.

객체의 생성과 객체 사이의 런타임 관계를 DI 관점에서 볼 때 컨테이너를 BeanFactory라고 한다.

BeanFactory에 여러가지 컨테이너 기능을 추가한 어플리케이션컨텍스트(ApplicationContext) 가 있다.


# BeanFactory와 ApplicationContext

## 1.  BeanFactory
- BeanFactory 계열의 인터페이스만 구현한 클래스는 단순히 컨테이너에서 객체를 생성하고 DI를 처리하는 기능만 제공한다.


- Bean을 등록, 생성, 조회, 반환 관리를 한다.

 

- 팩토리 디자인 패턴을 구현한 것으로 BeanFactory는 빈을 생성하고 분배하는 책임을 지는 클래스이다.

 

- Bean을 조회할 수 있는 getBean() 메소드가 정의되어 있다.

 

- 보통은 BeanFactory를 바로 사용하지 않고, 이를 확장한 ApplicationContext를 사용한다.

 

## 2.  ApplicationContext
- Bean을 등록, 생성, 조회, 반환 관리하는 기능은 BeanFactory와 같다.

 

- 스프링의 각종 부가 기능을 추가로 제공한다.

 

- BeanFactory 보다 더 추가적으로 제공하는 기능
    - 국제화가 지원되는 텍스트 메시지를 관리 해준다.
    - 이미지같은 파일 자원을 로드할 수 있는 포괄적인 방법을 제공해준다.
    - 리스너로 등록된 빈에게 이벤트 발생을 알려준다.

<br>

    따라서 대부분의 어플리케이션에서는 빈팩토리 보다는 어플리케이션콘텍스트를 사용하는 것이 더 좋다.