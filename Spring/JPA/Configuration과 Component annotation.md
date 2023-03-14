# configuration annotation은 뭔가
- 클래스를 빈으로 등록하기 위해서는 명시적으로 설정 클래스에서 @Bean 어노테이션을 사용해 수동으로 스프링 컨테이너에 빈을 등록하는 방법이 있다. 
- 설정 클래스는 다음과 같이 @Configuration 어노테이션을 클래스에 붙여주면 되는데, @Bean을 사용해 수동으로 빈을 등록해줄 때에는 메소드 이름으로 빈 이름이 결정된다. 그러므로 중복된 빈 이름이 존재하지 않도록 주의해야 한다.

```
@Configuration
public class ResourceConfig {

    @Bean
    public MangKyuResource mangKyuResource() {
        return new MangKyuResource();
    }

}
```

## Configuration 안에서 @Bean 이 빈으로 등록되는 과정

- @Configuration 안에서 @Bean이 빈으로 등록되는 과정은 간단하다. 스프링 컨테이너는 @Configuration이 붙어있는 클래스를 자동으로 빈으로 등록해두고, 해당 클래스를 파싱해서 @Bean이 있는 메소드를 찾아서 빈을 생성해준다.

- 하지만 어떤 임의의 클래스를 만들어서 @Bean 어노테이션을 붙인다고 되는 것이 아니고, @Bean을 사용하는 클래스에는 반드시 @Configuration 어노테이션을 활용하여 해당 클래스에서 Bean을 등록하고자 함을 명시해주어야 한다.

- 스프링 빈으로 등록된 다른 클래스 안에서 @Bean으로 직접 빈을 등록해주는 것도 동작은 한다. 하지만 @Configuration 안에서 @Bean을 사용해야 싱글톤을 보장받을 수 있으므로 @Bean 어노테이션은 반드시 @Configuration과 함께 사용해주어야 한다.

이러한 @Bean 어노테이션의 경우는 수동으로 빈을 직접 등록해줘야만 하는 상황인데, 주로 다음과 같을 때 사용한다.

1. 개발자가 직접 제어가 불가능한 라이브러리를 활용할 때
2. 애플리케이션 전범위적으로 사용되는 클래스를 등록할 때
3. 다형성을 활용하여 여러 구현체를 등록해주어야 할 때


# @Component
하지만 이렇게 수동으로 직접 빈을 등록하는 작업은 빈으로 등록하는 클래스가 많아질수록 상당히 많은 시간을 차지할 것이고, 생산력 저하를 야기할 것이다. 

그래서 스프링에서는 특정 어노테이션이 있는 클래스를 찾아서 빈으로 등록해주는 컴포넌트 스캔 기능을 제공한다.

    스프링은 컴포넌트 스캔(Component Scan)을 사용해 @Component 어노테이션이 있는 클래스들을 찾아서 자동으로 빈 등록을 해준다. 그래서 우리가 직접 개발한 클래스를 빈으로 편리하게 등록하고자 하는 경우에는 @Component 어노테이션을 활용하면 된다.

@Component를 갖는 어노테이션으로 @Controller, @Service, @Repository 등이 있으며 앞서 살펴봤던 @Configuration 역시 안에 @Component를 가지고 있다.

```
@Component
public @interface Configuration {

    boolean proxyBeanMethods() default true;

}
```

@Configuration 안에 있는 @Component에 의해 설정 클래스 역시 자동으로 빈으로 등록이 되고, 그래서 @Bean이 있는 메소드를 통해 빈을 등록해줄 수 있었던 것이다.

    스프링은 기본적으로 컴포넌트 스캔을 이용한 자동 빈 등록 방식을 권장한다. 또한 직접 개발한 클래스는 @Component를 통해 해당 클래스를 빈으로 등록함을 노출하는 것이 좋다. 

왜냐하면 해당 클래스에 있는 @Component만 보면 해당 빈이 등록되도록 잘 설정되었는지를 바로 파악할 수 있기 때문이다. @Bean을 이용하면 빈으로 등록하는 작업도 번거로우며 빈이 등록되었는지 파악하는 과정도 번거로워질 수 있다. 그러므로 위에서 @Bean으로 해야하는 경우를 제외한 대부분의 경우에 자동 등록 방식을 사용하면 된다.

추가로 @Component를 이용한다면 Main 또는 App 클래스에서 @ComponentScan으로 컴포넌트를 찾는 탐색 범위를 지정해주어야 한다. 하지만 SpringBoot를 이용중이라면 @SpringBootConfiguration 하위에 기본적으로 포함되어 있어 별도의 설정이 필요 없다.


# @Bean, @Configuration, @Component 차이 및 비교 요약

## @Bean, @Configuration
- 수동으로 스프링 컨테이너에 빈을 등록하는 방법
- 개발자가 직접 제어가 불가능한 라이브러리를 빈으로 등록할 때 불가피하게 사용
- 유지보수성을 높이기 위해 애플리케이션 전범위적으로 사용되는 클래스나 다형성을 활용하여 여러 구현체를 빈으로 등록 할 때 사용
- 1개 이상의 @Bean을 제공하는 클래스의 경우 반드시 @Configuration을 명시해 주어야 싱글톤이 보장됨

# @Component
- 자동으로 스프링 컨테이너에 빈을 등록하는 방법
- 스프링의 컴포넌트 스캔 기능이 @Component 어노테이션이 있는 클래스를 자동으로 찾아서 빈으로 등록함
- 대부분의 경우 @Component를 이용한 자동 등록 방식을 사용하는 것이 좋음
- @Component 하위 어노테이션으로 @Configuration, @Controller, @Service, @Repository 등이 있음