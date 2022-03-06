# Kotlin Array

### 배열의 인스턴스 생성

코틀린은 자바와 달리 배열을 생성해주는 arroyOf와 같은 팩토리 메소드를 제공한다. 그렇기 때문에 엄청 간결하게 배열 선언이 가능하다. 자바와 배열 접근 방법은 같지만 Kolin의 Array는 Class이다.

코드를 통해 arrayOf의 사용법에 대해 알아보자

```kotlin
val arr = arrayOf(0,1,2,3,4,5)
```

이런식으로 Array<Int>의 인스턴스를 쉽게 생성할 수 있다.

또한 잘 사용하지는 않겠지만 null로 배열을 채워서 인스턴스를 생성하는 메소드도 있다. 그건 바로 arrayOfNulls라는 메소드인데 nullable한 타입을 사용하고 싶을때 사용하는 것 같다. 

코드를 통해 arrayOfNulls의 사용법에 대해 알아보자

```kotlin
val arr = arrayOfNulls<String>(5)
```

배열의 인스턴스를 생성하는 방법이 또 있는데 그건 emptyArray 팩토리 메소드로 생성하는 것이다. Array 클래스에는 public 생성자가 하나만 있다. 이 생성자는 두 인자를 받는다. Int 타입의 size, (Int) → T 타입의 람다.

Array 클래스 생성자의 두 번째 인자인 람다는 배열을 생성할 떄 인덱스마다 호출된다. 예를들어 처음 5개의 정수를 제곱한 값의 문자열 배열을 생성하는 코드를 만들어보자

해당 코드는 다음과 같다.

```kotlin
val squareds = Array(5) { i -> (i*i).toString()}
```

해당 배열의 결과는 {”0”,”1”,”4”,”9”,”16”} 이다.

Array 클래스에는 squares[1]처럼 대괄호를 사용해 배열의 원소에 접근할 때 호출되는 public 연산자 메소드 get과 set이 정의되어 있다. 그래서 해당 index의 값을 가져오고 수정할 수 있다.

또한 코틀린에는 오토박싱과 언박싱 비용을 방지할 수 있는 기본 타입을 나타내는 클래스가 있다.

booleanArrayOf, byteArraof 등등 함수는 예상하는 것처럼 연관된 타입 배열을 생성한다.

> 코틀린에는 명시적인 타입은 없지만 값이 널 허용 값인 경우 생성된 바이트코드는 Integer와 Double과 같은 자바 래퍼 클래스를 사용하고, 널 비허용 값인 경우 생성된 바이트 코드는 int 와 double과 같은 기본 타입을 사용한다.
> 

### 배열의 Index 값 획득하기

주어진 배열의 적법한 인덱스 값을 알고 싶다면 Array의 indices 속성을 사용할 수 있다.

```kotlin
val arr = arrayOf(1,2,3,4,5)
val indices = arr.indices
```

일반적으로 배열을 순회할 떄 표준 for in 루프를 사용하지만 배열의 인덱스 값도 같이 사용하고 싶다면 withIndex 함수를 사용하자

```kotlin
fun <T> Array<out T>.withIndex() : Iterable<IndexedValue<T>>

data class IndexedValue<out T>(public val index : Int, public val value : T)
```

```kotlin
val arr = arrayOf(1,2,3,4,5,6)
for((index,value) in arr.withIndex()) {
	println("인덱스 : $index 값 : $value)
}
```

출력 결과

인덱스 :  0 값 : 1

인덱스 :  1 값 : 2

인덱스 :  2 값 : 3

인덱스 :  3 값 : 4

인덱스 :  4 값 : 5

인덱스 :  5 값 : 6

forEachIndexed 로도 대체 가능하다.