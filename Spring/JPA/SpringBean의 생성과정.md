# Bean의 개념
- Spring Bean은 Spring IoC 컨테이너가 관리하는 자바 객체로서 컨테이너에 의해 생명주기가 관리되는 객체를 의미한다. 
- IoC 컨테이너 안에 들어있는 객체로 필요할 때마다 IoC 컨테이너에서 가져와서 사용한다.
- 어노테이션인 @Bean을 사용하거나 xml 설정을 통해 일반 객체를 Bean으로 등록이 가능하다.
- 즉, Spring 에서는 Bean은 ApplicationContext가 알고 있는 객체이며 ApplicationContext가 생성하고 직접 관리해주는 객체를 의미한다.

<br>

# Bean 의 생명 주기
- 객체 생성 -> 의존 설정 -> 초기화 -> 사용 -> 소멸 과정의 생명 주기를 가지고 있다.
- Bean은 스프링 컨테이너에 의해 생명주기를 관리한다. 
- 스프링 컨테이너가 초기화될 때 먼저 빈 객체를 설정에 맞춰 생성하며 의존 관계를 설정한 뒤 해당 프로세스가 완료되면 빈 객체가 지정한 메소드를 호출해서 초기화를 진행한다.
- 객체를 사용해 컨테이너가 종료될 때 빈이 지정한 메서드를 호출해 소멸 단계를 거친다.
- 스프링은 InitializingBean 인터페이스와 DisposableBean을 제공하며 빈 객체의 클래스가 InitializingBean Interface 또는 DisposableBean을 구현하고 있으며 해당 인터페이스에서 정의된 메소드를 호출해 빈 객체의 초기화 또는 종료를 수행한다.
- 또한 어노테이션을 이용한 빈 초기화 방법에는 @PostConstruct와 빈 소멸에서는 @PreDestory를 사용한다.

<br>

# Spring Bean 의 Scope
- Bean Scope는 기본적으로 빈이 존재하는 범위를 뜻한다.
- Bean의 객체는 기본적으로 singleton의 범위를 가지며 singleton는 스프링 컨테이너의 시작과 종료까지 단 하나의 객체만을 사용하는 방식이다.
- request, session, global session의 scope는 일반 spring 어플리케이션이 아니라 Spring MVC Web Application에서만 사용된다.

<br>

|Scope|설명|
|------|----------|
|singleton|하나의 Bean 정의에 대해 Spring IoC Container에서 단 하나의 객체만 존재한다.|
|prototype|하나의 Bean 정의에 대해 다수의 객체가 존재할 수 있다.|
|request|하나의 Bean 정의에 대해 하나의 HTTP request의 생명주기 안에 단 하나의 객체만 존재한다. 각각의 HTTP request는 자신만의 객체를 가지며 Web-aware Spring ApplicationContext 안에서만 유효한 특징이 있다.|
|session|하나의 Bean 정의에 대해 하나의 HTTP Session의 생명주기 안에서 단 하나의 객체만 존재한다.  Web-aware Spring ApplicationContext 안에서만 유효한 특징이 있다.|
|global session|하나의 Bean 정의에 대해 하나의 global HTTP Session의 생명주기 안에서 단 하나의 객체만 존재한다. 일반적으로는 portlet context안에서만 유효하며,  Web-aware Spring ApplicationContext 안에서만 유효한 특징이 있다.|

- Bean의 객체 범위를 prototype으로 설정하면 객체를 매번 새롭게 생성한다는 특징이 있으며, prototype으로 설정하면 @Scope 어노테이션을 @Bean 어노테이션과 함께 사용해야 한다.