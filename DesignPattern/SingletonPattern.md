# Singleton Pattern

# 정의

    인스턴스가 필요할 때, 똑같은 인스턴스를 만들지 않고 기존의 인스턴스를 활용하는 것, 객체의 인스턴스가 오직 1개만 생성되는 패턴을 의미한다. 

## 사용하는 이유

객체는 생성할 때마다 메모리 영역을 할당받아야 한다. 최초 한 번의 new를 통해서 객체를 생성한다면 메모리 낭비를 방지할 수 있다. 

또한 싱글톤으로 구현한 인스턴스는 전역이므로, 다른 클래스의 인스턴스들이 데이터를 공유하는 것이 가능한 장점이 있다고 한다.
<br>

## 문제점

싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유시킬 경우 다른 클래스의 인스턴스들 간에 결합도가 높아져 “개방-폐쇄” 원칙을 위배하게 된다.

따라서 수정이 어려워지고 테스트하기가 어려워진다.

멀티스레드 환경에서 동기화처리를 안하면 인스턴스가 두 개 생성되는 경우가 발생할 수도 있다. 
<br>

## 멀티스레드에서 안전한 싱글톤 클래스, 인스턴스 만드는 방법

### 1. ****Lazy Initialization****

```java
public class ThreadSafeLazyInitialization{
 
    private static ThreadSafeLazyInitialization instance;
 
    private ThreadSafeLazyInitialization(){}
     
    public static synchronized ThreadSafeLazyInitialization getInstance(){
        if(instance == null){
            instance = new ThreadSafeLazyInitialization();
        }
        return instance;
    }
 
}
```

private static으로 인스턴스 변수 만듬

private으로 생성자를 만들어 외부에서의 생성을 막음

synchronized 동기화를 활용해 스레드를 안전하게 만듬

하지만 synchronized 는 성능저하를 발생시킴.

<br>

### 2. ****Lazy Initialization + Double-checked Locking****

```java
public class ThreadSafe_Lazy_Initialization{
    private volatile static ThreadSafe_Lazy_Initialization instance;

    private ThreadSafe_Lazy_Initialization(){}

    public static ThreadSafe_Lazy_Initialization getInstance(){
    	if(instance == null) {
        	synchronized (ThreadSafe_Lazy_Initialization.class){
                if(instance == null){
                    instance = new ThreadSafe_Lazy_Initialization();
                }
            }
        }
        return instance;
    }
}
```

1번과는 달리, 먼저 조건문으로 인스턴스의 존재 여부를 확인한 다음 두번째 조건문에서 synchronized를 통해 동기화를 시켜 인스턴스를 생성하는 방법

스레드를 안전하게 만들면서, 처음 생성 이후에는 synchronized를 실행하지 않기 때문에 성능저하 완화가 가능함
<br>

### 3. ****Initialization on demand holder idiom (holder에 의한 초기화)****

```java
public class Something {
    private Something() {
    }
 
    private static class LazyHolder {
        public static final Something INSTANCE = new Something();
    }
 
    public static Something getInstance() {
        return LazyHolder.INSTANCE;
    }
}
```

2번처럼 동기화를 사용하지 않는 방법을 안하는 이유는, 개발자가 직접 동기화 문제에 대한 코드를 작성하면서 회피하려고 하면 프로그램 구조가 그만큼 복잡해지고 비용 문제가 발생할 수 있음. 또한 코드 자체가 정확하지 못할 때도 많음

이 때문에, 3번과 같은 방식으로 JVM의 클래스 초기화 과정에서 보장되는 원자적 특성을 이용해 싱글톤의 초기화 문제에 대한 책임을 JVM에게 떠넘기는 걸 활용함

클래스 안에 선언한 클래스인 holder에서 선언된 인스턴스는 static이기 때문에 클래스 로딩시점에서 한번만 호출된다. 또한 final을 사용해서 다시 값이 할당되지 않도록 만드는 방식을 사용한 것

# 추가

# ENUM을 사용한 방법

```
public enum EnumSingleton {
    uniqueInstance;
}
```

리플렉션을 통해 싱글톤을 깨트리는 공격에 안전
직렬화 보장

성능 문제 : Eager-Initialization로 인한 문제와 싱글톤이 Enum 외의 클래스를 상속해야 하는 경우에는 사용할 수 없다.

또한 안드로이드 같이 Context 의존성이 존재하는 환경일 경우 싱글턴 초기화시 Context라는 의존성이 끼어들 가능성이 있다.


장단점

LaszHolder : 성능이 중요시 되는 환경

Enum : 직렬화, 안정성 중요시 되는 환경
 