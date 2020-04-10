# 자바 관련 질문
### Q. `final` 사용하는 이유
자바에서 final은 단 한 번만 할당할 수 있도록 제한해주는 키워드입니다. 그리고 클래스, 메서드, 변수에 선언함에 따라 각각 특징을 가지고 있습니다.
- 클래스에 선언하는 경우, 해당 클래스를 다른 클래스가 상속하려는 경우 컴파일 에러가 발생합니다.
- 메서드에 선언하는 경우, 해당 메서드는 오버라이드를 할 수 없습니다.
- 변수에 선언하는 경우, 해당 변수를 불변으로 만들어줍니다. 만약 값을 변경하려고 하면 컴파일 에러가 발생합니다.

### Q. Error와 Exception의 차이
Error는 애플리케이션이 아닌 시스템 수준에서의 비정상적인 상황입니다. 메모리가 부족하거나 널 참조와 같이 시스템에 문제가 생긴 경우에는 애플리케이션에서는 복구할 수 없는 심각한 상황입니다. Exception은 애플리케이션에서 발생하는 비정상적인 상황으로 대부분 개발자가 처리할 수 있는 영역입니다. Exception은 또 다시 checked exception과 unchecked exception으로 나뉩니다.

#### 꼬리질문 1. Checked Exception과 Unchecked Exception 차이
Checked Exception은 컴파일 시간에 확인하는 예외로 반드시 try/catch나 throw로 처리해야합니다. 반면에 unchecked exception은 런타임 시간에 확인하므로 코드상으로 처리를 하지 않아도 컴파일 에러가 발생하지 않습니다. 또 다른 차이점은 트랜잭션의 roll-back 여부입니다. checked exception은 예외 발생시 트랜잭션을 롤백하지 않습니다. 반면에 unchecked exception은 롤백을 수행합니다.

#### 꼬리질문 2. Custom Exception을 RuntimeException으로 상속한 이유

### Q. `String`과 `StringBuilder` 차이
Java에서 String은 불변 객체이므로, 더하기 연산을 할 때 기존 메모리를 제거하고, 새로운 메모리를 할당해야하여 비효율적으로 동작합니다. 반면에 StringBuilder는 mutable하여 문자열을 기존의 메모리 뒤에 붙일 수 있어 더 효율적인 더하기 연산이 가능합니다. 최근 자바는 String 역시 간단한 +연산은 메모리를 제거, 재할당이 아닌 효율적인 방식으로 동작한다고 알고 있습니다.

### Q. `StringBuilder`와 `StringBuffer` 차이
StringBuffer는 내부적으로 동기화(synchronized)되어 있어 멀티 스레드 환경에서 정상적으로 동작합니다. 반면에 StringBuilder는 thread-safe하지 않습니다.

### Q. Java8에서 추가된 기능
#### 꼬리질문 1. Lambda
람다는 자바8 버전에 도입된 것으로서, 기존의 익명 클래스를 간단하게 사용하기 위해 만들어진 문법입니다. 익명 클래스는 주로 재사용할 필요 없는 로직을 일회성으로 사용하기 위해 매개변수에 바로 선언하는 방법입니다. 이는 override와 같은 여러 정해진 규칙이 있어 이를 제거하여 간단히 표현하는  것이 람다식입니다. 그리고 자바8에서는 람다의 재사용을 위해 functional interface라는 추상 메서드가 단 하나인 인터페이스를 정의하였습니다. 이 인터페이스를 매개변수로 가지고 있다면, 사용하는 쪽은 각각 필요한 람다식을 정의하여 사용할 수 있습니다.

#### 꼬리질문 2. Stream API
#### 꼬리질문 3. Optional
#### 꼬리질문 4. Date Type

### Q. 롬복이 생성하는 메서드가 어느 시점에서 생성되는 지

### Q. HashMap의 시간 복잡도

### Q. equals and hashcode 재구현한 이유

### Q. JVM
- [VM의 장단점]
- [객체가 JVM 메모리에 저장될 때 어디에 저장되는지?]
- [Java의 세 가지 변수에 대해 JVM 메모리와 연관지어 설명해주세요.]
### Q. GC
### Q. JUnit의 생명주기에 대해 아는지?
### Q. Generic을 쓰는 이유는?