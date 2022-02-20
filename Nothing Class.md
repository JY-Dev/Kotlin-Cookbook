# Nothing Class

코틀린에는 Nothing이라는 이름의 클래스가 있다. Nothing 클래스의 전체 구현은 아래와 같다

```kotlin
public class Nothing private constructor()
```

private 생성자는 클래스 밖에서 인스턴스화할 수 없다는 것을 의미하고, 보다시피 클래스 안쪽에서도 인스턴스화하지 않는다. 따라서 Nothing의 인스턴스는 존재하지 않는다. 코틀린 공식 문서에서도 결코 존재할 수 없는 값을 나타내기 위해 Nothing을 사용할 수 있다고 명시되어있다.

그러면 Nothing은 어떤경우에 사용할까? 다음 예제를 보자

```kotlin
fun doNothing() : Nothing = throw Exception("Nothing")
val nothing = null
```

메소드는 리턴 타입을 반드시 구체적으로 명시해야하는데 doNothing 메소드는 결코 리턴하지 않으므로 리턴 타입은 Nothing이다. 또한 nothing 변수는 널 할당이 가능한 타입이고 nothing에 대한 다른 정보가 없기 때문에 추론된 타입은 Nothing?이다. 코틀린에서의 Nothing class는 실제로 다른 모든 타입의 하위 타입이다.

Nothing class가 다른 모든 타입의 하위 타입이 되어야 하는 이유를 살펴보기 위해 아래와 같은 코드처럼 if 문이 예외를 던질 수 있다고 가정하자

```kotlin
val x = if(Random.nextBoolean()) "true" else throw Exception("exception")
```

x의 추론 타입은 Random.nextBoolean 함수가 생성하는 불리언 값이 참인 경우에 할당되는 문자열에 따라 String, Comparable<String>, CharSequence,Serializeable , 또는 Any일 수도 있다. 이코드의 else 절은 Nothing을 리턴하고 Nothing은 모든 타입의 하위 타입이므로 최종 리턴 타입은 Nothing이 아닌 다른 타입이 된다. 그래서 해당 변수의 타입은 boolean이 된다.