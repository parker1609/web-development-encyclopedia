# 디자인 패턴 관련 질문

## Q. Singleton Pattern
싱글톤 패턴은 애플리케이션에서 하나의 객체만을 생성하여 사용하는 패턴입니다. 메모리에 한 번 할당된 객체는 다른 곳에서 생성하려하면 새로 메모리를 할당하지 않고 이미 할당된 객체의 정보를 반환하도록 합니다.

싱글톤 패턴의 장점은 객체를 사용할 때마다 생성하고 삭제하는 비용이 없고 전역 변수처럼 어느 곳에서나 같은 객체를 사용할 수 있습니다. 이러한 장점으로 서버 애플리케이션에서 상태를 가지지 않는 공통의 객체가 필요할 때 사용합니다.(ex, Spring Bean) 그 외에도 데이터베이스의 커넥션풀, 스레드풀, 캐시 등에 사용됩니다.

반면에 싱글톤 패턴의 단점은 여러 쓰레드에서 같은 객체를 공유하므로 동기화 문제에 신경써주어야 합니다. 싱글톤으로 사용할 객체는 내부 인스턴스 변수처럼 race condition이 발생할 수 있는 문제점을 생각해야 합니다. 그리고 모두가 공유하고 있는 객체라는 것은 의존성이 매우 복잡해지고 많은 책임을 가질 수 있습니다. 이는 변경에 대한 비용이 점점 커지며 객체지향 설계에서 멀어질 수 있습니다.

싱글톤 객체는 기본적으로 멀티 쓰레드 환경에서 안전하게 생성되어야 합니다. 가장 많이 사용하는 Holder에 의한 초기화 방법은 다음과 같습니다.

```java
public class SingletonObject {
    private SingletonObject() {
    }

    private static class LazyHoler {
        public static final SingletonObject INSTANCE = new SingletonObject();
    }

    public static SingletonObject getInstance() {
        return LazyHoler.INSTANCE;
    }
}
```


## Q. Strategy Pattern
전략 패턴은 런타임 환경에서 동적으로 객체를 다른 객체로 변경하면서 기능의 확장을 코드의 변경없이 할 수 있는 패턴입니다. 이 패턴은 객체지향 프로그래밍의 SOLID 원칙 중 개방-폐쇄 원칙을 지키는 가장 대표적인 방법입니다.

전략 패턴을 구현하기 위해서는 3가지 역할이 필요합니다.
1. 클라이언트
2. 클라이언트가 사용할 전략
3. 클라이언트와 클라이언트가 사용할 객체를 이어줄 제 3자

위 세 가지 역할을 하는 객체가 필요하며 두 번째인 클라이언트가 사용할 전략(객체)은 여러 객체로 변경가능해야 하므로 클래스 상속 또는 인터페이스 추상화가 된 객체어야 합니다.

간단하게 계산기 프로그램을 살펴봅시다.
- 클라이언트: `Calculator` 객체의 `calculate()` 메서드
- 클라이언트가 사용할 전략: `Operator` 인터페이스
    - 더하기: `PlusOperator`
    - 빼기: `MinusOperator`
- 제 3자: `main()` 메서드

#### 1. 클라이언트

```java
public class Calculator {
    public int calculate(int a, int b, Operator operator) {
        return operator.execute(a, b);
    }
}
```

#### 2. 클라이언트가 사용할 전략

```java
public interface Operator {
    int execute(int a, int b);
}

public class PlusOperator implements Operator {
    @Override
    public int execute(int a, int b) {
        return a + b;
    }
}

public class MinusOperator implements Operator {
    @Override
    public int execute(int a, int b) {
        return a - b;
    }
}
```

#### 3. 제 3자

```java
public static void main(String[] args) {
    Calculator calculator = new Calculator();
    int a = 10;
    int b = 5;
    int result;

    // 더하기
    result = calculator.calculate(a, b, new PlusOperator());

    // 빼기
    result = calculator.calculate(a, b, new MinusOperator());
}
```

여기서 중요하게 살펴볼 점은 클라이언트는 여러 기능을 제공하는데 있어서 `if-else`문을 사용하지도 변경이 필요하지도 않다는 것입니다. 여기서 만약 곱셈과 나눗셈을 추가하고 싶으면 단지 `MultipleOperator`, `DivideOperator` 객체를 구현하여 제 3자에서 삽입해주기만 하면 됩니다.

전략 패턴은 매우 많은 곳에서 살펴볼 수 있습니다. Spring Framework의 IoC/DI가 대표적입니다.


## Q. Factory Patterns
- <https://woowacourse.github.io/javable/2020-05-26/static-factory-method>

## Q. Template Method Pattern

## Q. Facade Pattern

## Q. Builder Pattern

## Q. Decorator Pattern

## Q. Observer Pattern



