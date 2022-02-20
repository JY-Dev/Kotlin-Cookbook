# 사용자 정의 획득자(getter)와 설정자(setter) 생성하기

다른 객체 지향 언어처럼 코틀린 클래스는 데이터와 보통 캡슐화로 알려진 해당 데이터를 조작하는 함수로 이뤄진다. 코틀린은 특이하게도 모든 것이 기본적으로 public 이다. 이는 데이터 은닉 원칙을 침해하는 것 처럼 보인다. 코틀린은 이러한 딜레마를 특이한 방법으로 처리한다. 코틀린 클래스에서 필드는 직접 선언할 수 없다.

```kotlin
class Person(val name : String){
	val age = 20
}
```

위와 같은 코드를 보면 왜 코틀린 클래스에서 필드를 직접 선언할 수 없다는 말이 이상하게 들린다.

Person Class는 name과 age라는 두가지 속성을 정의한다. 속성하나는 주 생성자 안에 선언된 반면에 다른 속성은 클래스의 최상위 멤버로 선언되었다. 물론 두속성 모두 주생성자에서 선언할 수 있지만 속성을 정의하는 다른 대안 문법을 보여주기 위해서 따로 정의하였다. 최상위 멤버로 age를 선언할때의 특징은 apply를 통해 age에 값을 할당할 수 있지만 클래스를 인스턴스화 할때 age에 값을 할당할 수 없다는 것이다. 또한 쉽게 getter와 setter를 추가할 수 있다는 것이 특징이다.

```kotlin
var <propertyName>[: <PropertyType] [= <property_init]
		[<getter>]
		[<setter>]
```

속성을 정의하는 전체 문법은 위와 같다. 속성 초기화 블록 , getter , setter는 사용자의 선택사항이다. 속성 타입이 초기값 또는 획득자의 리턴 타입에서 추론 가능하다면 속성 타입 또한 선택사항이다. 하지만 생성자에서 선언한 속성에서는 타입 선언이 필수다.

getter를 정의하는 방법에 대해 알아보자

```kotlin
val isLowPriority
	get() = priority < 3
```

위와 같은 코드는 앞서 설명한 바와 같이 isLowPriority의 타입은 get 함수의 리턴 타입으로 부터 추론되어 설정된다. 

setter를 정의하는 방법에 대해 알아보자

```kotlin
val priority = 3
	set(value) {
		field = value.coerceIn(1..5)
	}
```

setter에서는 field 식별자가 코틀린이 생성한 지원 필드를 참조하는 데 사용됐다. field 식별자는 오직 setter에서나 getter에서만 사용할 수 있다. 이러한 getter setter는 private과 같은 변경자를 통해 제한 시킬수도 있다.