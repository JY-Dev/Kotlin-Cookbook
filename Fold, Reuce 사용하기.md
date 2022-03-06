# Fold , Reduce 사용하기

### Fold 함수

fold 함수는 배열 또는 반복 가능한 컬렉션에 적용할 수 있는 축약 연산이다.

fold 함수의 문법은 다음 과 같다

```kotlin
inline fun <R> Iterable<T>.fold(
initial : R,
operation : (acc:R,T) -> R
) : R
```

똑같은 함수가 Array를 비롯한 IntArray, DoubleArray 등의 명시적 타입 배열에 정의돼 있다.

fold는 두가지 인수를 받게되는데 initial, operation을 받는다. 첫 번째는 초기값이며 두 번째는 두 개의 인자를 받아 누적자를 위해 새로운 값을 리턴하는 함수이다.

Fold로 구현한 각각의 값을 출력하는 sum 함수를 보자

```kotlin
arrayOf(3,1,4,1,5,9).sumFold()

fun Array<Int>.sumFold() =
	fold(0) { acc, n ->
		println("acc = $acc, n = $n")
		acc+n
} 
```

일단 출력을 보면

acc = 0 , n= 3

acc = 3 , n= 1

acc = 4 , n= 4

acc = 8 , n= 1

acc = 9 , n= 5

acc = 14 , n= 9

초기 acc값을 보면 initail로 설정해주는 0으로 출력되는 걸 볼 수 있다. 그리고 n은 배열의 첫번째 값을 나타낸다. 그 다음을 보면 초기 값인 0과 배열의 첫번째 값이 합해진 값이 acc로 출력되는 걸 볼 수 있다. 즉 acc + n 이 새로운 acc값이 되는걸 볼 수 있다.

확실히 람다로 전달받아 계산하기 때문에 불변성이라는 이점이 생겨 아주 좋은 함수인것 같다. 반복적으로 수행하는 배열에 대한 로직이 있을 때 fold로 사용하는 연습을 해봐야겠다는 생각이 들었다. 

### Reduce 함수

reduce 함수 같은 경우는 fold와 많이 비슷해보이지만 초기값을 설정해주지 않아도 동작하는 함수이다. 그래서 fold와 다르게 초기값을 설정해주고 싶지 않을때 사용하는 함수이다.

reduce함수의 코드를 한번 봐보자

```kotlin
public inline fun IntArray.reduce(
	operation : (acc : Int, Int) -> Int {
	if(isEmpty())
		throw UnSupportedOperationException("Empty array can't be reduced.")
	var accumlator = this[0]
	for (index in 1..lastIndex) {
		accumlator = operation(accumlator, this[index])
	}
	return accumlator
}
```

fold와 같이 operation를 받는데 용도는 똑같다 누적자를 위해 두개의 인자를 받아 어떤 처리를 해주고 두개의 인자와 같은 인자를 return 해준다. fold 와 다른점이 있는데 초기값을 직접 설정해주지 않는 대신 reduce 함수 내부에서 초기값을 설정해 주는데 배열의 첫번째 값을 초기값으로 사용한다. 

그대신 배열이 비어있다면 UnSupportedOperationException 예외를 던진다.

fold 와 같이 sum함수를 구현해보자.

```kotlin
arrayOf(3,1,4,1,5,9).sumReduce()

fun Array<Int>.sumReduce() = 
		reduce { acc, i -> acc + i }
```

이런식으로 operation만 구현하면 sum함수를 간단하게 구현할 수 있다.

reduce를 사용하면서 주의해야 하는 점이 있는데 예를 들어 모든 입력 값을 서로 더하기 전에 모든 입력 값을 수정하고 싶다고 해보자. 입력 값을 서로 더하기 전에 각각의 입력 값을 두배로 증가시키고 싶어서 다음과 같이 구현하는 경우가 있는데 한번 봐보자

```kotlin
arrayOf(3,1,4,1,5,9).sumReduceDouble()

fun Array<Int>.sumReduceDouble() = 
		reduce { acc, i -> acc + i*2 }
```

출력 결과를 보자면

acc = 3 , n= 1

acc = 5 , n= 4

acc = 13 , n= 1

acc = 15 , n= 5

acc =25 , n= 9

이렇게 된다. 초기값은 리스트의 첫번째 값인 3로 설정되서 구해지기 때문에 원하는 결과를 얻을 수 없다. 그렇기 때문에 이럴 경우에는 fold를 사용하자