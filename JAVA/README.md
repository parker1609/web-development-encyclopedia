# 자바 관련 질문
## Q. `final` 키워드에 대해 설명하시오.
자바에서 final은 단 한 번만 할당할 수 있도록 제한해주는 키워드입니다. 그리고 클래스, 메서드, 변수에 선언함에 따라 각각 특징을 가지고 있습니다.
- 클래스에 선언하는 경우, 해당 클래스는 다른 클래스가 상속할 수 없습니다.
- 메서드에 선언하는 경우, 해당 메서드는 오버라이드를 할 수 없습니다.
- 변수에 선언하는 경우, 해당 변수를 불변으로 만들어줍니다.

만약 이를 시도할 경우 컴파일 에러가 발생합니다.

## Q. Error와 Exception의 차이에 대해 설명하시오.
Error는 애플리케이션이 아닌 시스템 수준에서의 비정상적인 상황입니다. 메모리가 부족하거나 널 참조와 같이 시스템에 문제가 생긴 경우는 복구할 수 없는 심각한 상황입니다. 

Exception은 애플리케이션에서 발생하는 비정상적인 상황으로 대부분 개발자가 처리할 수 있는 영역입니다. 자바에서 Exception은 또 다시 checked exception과 unchecked exception으로 나뉩니다.

### 꼬리질문 1. Checked Exception과 Unchecked Exception 차이에 대해 설명하시오.
Checked Exception은 컴파일 시간에 확인하는 예외로 반드시 try/catch나 throw로 처리해야합니다. 트랜잭션 범위 내부에서 checked exception이 발생한 경우 roll-back을 수행하지 않습니다.

Unchecked exception은 런타임 시간에 확인하는 예외로 코드상으로 처리를 하지 않아도 컴파일 에러가 발생하지 않습니다. unchecked exception이 트랜잭션 범위 내에서 발생할 시 roll-back을 수행합니다.

## Q. `String`과 `StringBuilder` 차이에 대해 설명하시오.
Java에서 String은 불변 객체이므로, 더하기 연산을 할 때 기존 메모리를 제거하고 더한 만큼 새로운 메모리를 할당해야하므로 비효율적입니다.

StringBuilder는 문자열을 기존의 메모리 뒤에 붙일 수 있어 더 효율적인 더하기 연산이 가능합니다. 

최근 자바는 String 역시 간단한 더하기 연산은 메모리를 제거 및 재할당이 아닌 효율적인 방식으로 동작한다고 알고 있습니다.

## Q. `StringBuilder`와 `StringBuffer` 차이에 대해 설명하시오.
StringBuffer는 내부적으로 동기화(synchronized)되어 있어 멀티 스레드 환경에서 안정적으로 동작합니다. 이는 내부적으로 synchronized 키워드를 통해 동기화 로직이 추가되어있습니다.

StringBuilder는 thread-safe 하지 않습니다.

멀티 스레드에서 공유하는 문자열인 경우 StringBuffer를 사용하고, 단일 스레드이거나 공유하지 않는다면 StringBuilder를 사용하는 것이 좋습니다. 동기화를 수행하는 것은 비용이 발생하므로 동기화를 할 필요 없는 환경에서는 더 효율적인 StringBuilder를 사용합니다.

## Q. Java8에 대해 설명하시오.
자바8에서 추가된 기능은 대표적으로 람다, 스트림 API, Optional 등이 있습니다. 
(그 외에 default 키워드, computable future, LocalDateTime 등이 있음.)

### 꼬리질문 1. Lambda에 대해 설명하시오.
람다는 자바8 버전에 추가된 기능으로서, 동작 파미터화를 하는 방법 중 하나입니다.
동작 파라미터화는 매개변수에 로직을 수행하는 메서드를 저장하는 것입니다. 초기에는 생성된 객체를 단순히 매개변수로 전달하는 방법을 사용했고, 그 이후에 익명 클래스를 사용하였습니다. 람다는 익명 클래스를 간단히 표현하는 방법입니다.

### 꼬리질문 1-1. 익명 클래스나 람다를 사용하는 이유에 대해 설명하시오.
익명 클래스나 람다를 사용하는 이유는 재사용할 필요없는 일회성 로직을 처리할 때 유용합니다. 이러한 일회성을 위해 불필요한 클래스 파일이 늘어나는 문제점을 해결할 수 있습니다.

#### 람다가 생긴 과정

```java
public interface ApplePredicate {
    boolean test(Apple apple);
}

public static List<Apple> filterApples(List<Apple> inventory, ApplePredicate p) {
    // ...
}
```

위 예제를 통해 람다가 생긴 과정에 대해 설명하겠습니다. 먼저 `ApplePredicate`는 여러 유사한 동작을 인터페이스로 추상화한 것입니다. 그리고 이를 매개변수로 사용하는 것이 동작 파라미터화입니다.

1. 인터페이스 추상화를 통한 동작 파라미터화

```java
List<Apple> heavyApples = FilteringApples.filterApples(inventory, new AppleHeavyWeightPredicate());
List<Apple> greenApples = FilteringApples.filterApples(inventory, new AppleGreenColorPredicate());
```

초기에는 단순히 구현 클래스 파일을 만들어 사용했습니다. 재사용할 필요없는 로직을 클래스 파일로 만드는 것은 비효율적인 작업입니다.

2. 익명 클래스를 통한 동작 파라미터화

```java
List<Apple> heavyApples = FilteringApples.filterApples(inventory, new ApplePredicate() {
    @Override
    public boolean test(Apple apple) {
        return apple.getWeight() > 150;
    }
});
List<Apple> greenApples = FilteringApples.filterApples(inventory, new ApplePredicate() {
    @Override
    public boolean test(Apple apple) {
        return "green".equals(apple.getColor());
    }
});
```

다음은 클래스 파일을 따로 만들지 않고 익명 클래스를 사용한 것입니다. 여기서는 `@Override`와 같은 정형화된 형식이 있습니다. 이를 반복하는 것은 매우 피곤한 작업이 될 것입니다.

3. 람다를 통한 동작 파라미터화

```java
List<Apple> heavyApples = FilteringApples.filterApples(inventory, 
    (Apple apple) -> apple.getWeight() > 150);
List<Apple> greenApples = FilteringApples.filterApples(inventory,
    (Apple apple) -> "green".equals(apple.getColor()));
```

람다는 익명 클래스의 여러 정형화된 형식을 생략하여 간단히 사용할 수 있습니다. 하지만 이를 남발하는 것에는 가독성이 안좋아진다는 평가도 있습니다.

### 꼬리질문 1-2. 메서드 래퍼런스
메서드 래퍼런스는 람다를 축약한 형태로, 메서드 이름을 명시적으로 나타내어 가독성을 높여줍니다.

메서드 레퍼런스는 총 세 가지로 구분할 수 있습니다.
1. 정적 메서드 레퍼런스
2. 다양한 형식의 인스턴스 메서드 레퍼런스
3. 기존 객체의 인스턴스 메서드 레퍼런스

![method_reference](./images/java8_method_reference.png)

### 꼬리질문 2. Stream API에 대해 설명하시오.
스트림은 java8에서 컬렉션 데이터를 선언형으로 처리하는 API입니다. 선언형은 SQL 질의어와 같이 내부 동작이 어떻게 동작하는지 신경쓰지 않고 무엇을 하는지를 직접적으로 표현합니다.

스트림 API를 사용함으로써 세 가지 장점을 얻을 수 있습니다.
1. 선언형이므로 간결하고 가독성을 높일 수 있습니다.
2. map, collet와 같은 여러 API를 제공하며, 이는 결과로 스트림을 반환하기 때문에 이를 조립하며 사용할 수 있습니다.
3. 병렬을 처리해주는 기능을 제공하여 좀 더 안전하게 성능을 높일 수 있습니다.

스트림 API의 특징은 두 가지가 있습니다.
1. 파이프라이닝이 가능합니다. 스트림 연산은 스트림을 반환할 수 있기 때문에 이를 연결해서 파이프라인을 구성할 수 있습니다.
2. 기존의 for문이 외부 반복을 하는 것과는 달리 내부 반복을 수행합니다.

### 꼬리질문 2-1. 스트림과 컬렉션의 차이에 대해 설명하시오.
스트림과 컬렉션의 차이는 세 가지가 있습니다.
1. 데이터를 언제 계산하는지입니다. 컬렉션에 추가되는 데이터는 이미 모든 계산이 끝난 상태입니다. 반면에 스트림은 요청할 때 데이터를 계산하여 추가할 수 있습니다.
2. 스트림은 단 한번만 탐색할 수 있습니다. 이전의 데이터를 다시 탐색하고 싶다면 같은 데이터로 새로운 스트림을 만들어야 합니다.
3. 컬렉션은 외부 반복을 사용하고, 스트림은 내부 반복을 수행합니다.

### 꼬리질문 3. Optional에 대해 설명하시오.
Optional은 널의 위험성을 해결하기 위해 객체를 한 번더 캡슐화하는 클래스입니다. Optional은 객체가 비어있다면 null이 아닌 Optional.empty()라는 메서드를 반환합니다. Optional을 사용함으로써 해당 객체가 없을 수 있다는 것을 명시적으로 알려줄 수 있습니다.

### 꼬리질문 3-1. NULL때문에 발생하는 문제점에 대해 설명하시오.
자바에서 NULL의 가장 큰 문제는 NullPointException이라고 생각합니다. 이는 NULL인 객체를 참조하려고 할 때 발생합니다. 그리고 NULL이라는 것은 값이 없음을 의미할 수도 있고, 개발자의 실수일 수도 있습니다. 따라서 어떤 의미인지 알기 힘듭니다.

### 꼬리질문 4. Date Type에 대해 설명하시오.
자바8이전의 시간 타입은 특정 날짜로부터 오프셋으로 사용하고, 쓰레드에 안전하지 않는 등의 문제점이 많았습니다. 그래서 서드파티 라이브러리를 사용했었는데, 자바8에서는 이를 해결하고자 LocalData, LocalTime과 같은 타입을 제공하고 있습니다.

## Q. Collection에 대해 설명하시오.
컬렉션은 데이터를 다루는 여러 자료구조에 대한 인터페이스를 만들어 데이터를 효율적으로 조작하기 위함입니다. 상위 인터페이스로는 List, Map, Set이 존재하며, 이를 상속하여 기능에 따라 여러 구현체가 존재합니다.

![collection](./images/collection_structure.png)

### 꼬리질문 1. HashMap 시간복잡도
HashMap은 해시 함수를 사용한 Map 자료구조입니다. 해시를 사용하므로 충돌이 발생할 수 있는데, 자바8 이전에는 Linked List로 관리하여 최악 시간복잡도는 O(N)이었습니다. 자바8 이후로는 Linked List가 아닌 Balanced Tree를 사용하므로 최악의 시간복잡도는 O(logN)으로 성능이 향상되었습니다.


## Q. 롬복(Lombok)이 생성하는 메서드가 어느 시점에서 생성되는 지
롬복은 AnnotationProcessor을 이용하는데, 이는 컴파일 시점에 Annotation별로 코드를 생성하는 역할을 합니다. 따라서 롬복 역시 컴파일 시점에 생성됩니다.

## Q. 추상 클래스(abstract class)와 인터페이스(interface)에 대해 설명하시오.
추상 클래스와 인터페이스 둘 다 상속이라고 말하는 상위 클래스의 기능을 하위 클래스에서 재정의하기 위해 사용하는 방법입니다. 메서드를 재정의하기 위해서 abstract 키워드를 사용하여 추상 메서드로 선업합니다. 이 메서드는 상위 클래스에서는 구현하지 않고, 하위 클래스에서 override하여 구현합니다.

추상 클래스는 추상 메서드와 일반 메서드가 섞여있는 클래스입니다. 추상 메서드가 하나 이상있는 클래스는 abstract를 클래스 앞에 선언해주어야 합니다.

인터페이스는 추상 메서드만으로 이루어진 클래스를 말합니다. 내부 구현이 없기 때문에 인터페이스는 다중 상속이 가능합니다.

### 꼬리질문 1. 추상 클래스와 인터페이스를 구분하는 이유
추상 클래스는 상하위 사이에서 공통적인 상태와 행동을 가지는 경우에 사용했습니다. Is-A 관계가 성립할 때입니다. (+실제 사용한 예시)

인터페이스는 구현없이 추상 메서드만 가지므로 높은 추상화가 가능합니다. 추상 클래스와 달리 어떠한 관계인지 상관없이 공통 부분을 추출할 수 있습니다.

추상 클래스와 인터페이스 모두 공통 부분을 추출하여 중복을 제거하고, 하나의 타입으로 묶어 다형성을 실현할 수 있습니다.

## Q. JVM
- [VM의 장단점]
- [객체가 JVM 메모리에 저장될 때 어디에 저장되는지?]
- [Java의 세 가지 변수에 대해 JVM 메모리와 연관지어 설명해주세요.]
## Q. GC
## Q. JUnit의 생명주기에 대해 아는지?


## Q. Generic을 쓰는 이유는?
제네릭은 일반화된 타입을 선언하여, 사용하는 곳에서 타입을 컴파일 시간에 지정할 수 있도록 합니다.

### 꼬리질문 1. 제네릭의 장점
1. 일반화된 타입을 사용하므로 타입 변환이나 검사를 할 필요 없이 유연성있는 코드를 만들 수 있습니다.
2. 컴파일 시간에 타입이 지정되므로, 미리 오류를 잡을 수 있어 안정성을 확보할 수 있습니다.

## Q. 개인적인 경험에 의존하는 질문
- Custom Exception을 RuntimeException으로 상속한 이유
- equals and hashcode 재구현한 이유
