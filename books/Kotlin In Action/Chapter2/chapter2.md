# 2장 코틀린 기초

## 1. 함수
```kotlin
fun main(args: Array<String>) {
    println("Hello, world!")
}

fun max (a: Int, b: Int) : Int {
    return if (a > b) a else b
}
```

위의 코드를 통해 간단하게 코틀린의 문법에 대해 알아보자.
- 함수를 선언할 때 fun 키워드를 사용한다.
- parameter 이름 뒤에 type을 쓴다.
- 함수를 (자바와 달리) 클래스 안에 넣을 필요없이 최상위 수준에 정의할 수 있다.
- 코틀린 표준 라이버르리는 여러가지 자바 표준 라이브러리 함수를 간결하게 사용할 수 있도록 wrapper를 제공한다. (e.g println)
- 최신 프로그래밍 언어 경향과 마찬가지고 줄 끝에 세미골론(;)을 붙이지 않아도 좋다.
- 반환 타입은 parameter 목록의 닫는 괄호 다음에 오며, 괄호와 반환 타입 사이를 콜론(:)으로 구분해야 한다.

위의 예제에서 흥미로운 부분은 코틀린에서의 if는 값을 만들어 내지 못하는 statement가 아닌 expression이라는 점이다.
> **문(statement)과 식(expression)의 구분**
코틀린에서 if는 **"문"** 이 아닌 **"식"** 이다. 
**차이:**  **"식"** 은 값을 만들어 내며 다른 식의 하위 요소로 계산에 참여할 수 있는 반면 **"문"** 은 자신을 둘러싸고 있는 가장 안쪽 블록의 최상위 요소로 존재하며 아무런 값을 만들어 내지 않는다.
자바에서는 모든 제어 구조가 **"문"** 인 반면 코틀린에서는 루프를 제외한 대부분의 제어 구조가 **"식"** 이다.

- 위의 max 함수를 예로 **"식이 본문인 함수"** 와 **"블록이 본문인 함수**" 에 대해 알아보자.
	- max 함수의 본문은 if 식 하나로만 이루어져 있다. 이런 경우 다음과 같이 중괄호를 없애고 return을 제거하면서 등호(=)를 식 앞에 붙이면 더 간결하게 함수를 표현할 수 있다.
	```kotlin
  fun max(a: Int, b: Int) : Int = if (a > b) a else b
  ``` 
  이 처럼 등호와 식으로 이루어진 함수를 **"식이 본문인 함수"** 라 부르고, 앞의 예와 같이 본문이 중괄호로 둘러싸인 함수를 **"블록이 본문인 함수"** 라고 부른다.
  
  위의 max함수에서 반환타입을 생략할 수도 있는데 이는 컴파일러가 타입 추론(type inference)을 통해 프로그램 구성 요소의      타입을 정해주기 때문이다.
  다만 유의할 점은 반환 타입의 생략은 **"식이 본문인 함수"** 의 반환 타입만 생략 가능하다는 점이다.
  **"블록이 본문인 함수"** 가 값을 반환한다면 반드시 반환타입을 지정하고 return 문을 사용해 반환 값을 명시해야한다.
  

## 2. 변수
#### 2.1. 코틀린의 변수 선언
	```kotlin
	val answer = 42
	val answer: Int = 42

	val answer: Int //초기화 식을 사용하지 않고 변수를 선언하려면 type을 반드시 명시해야함!
	answer = 42;
	```
	이런식으로 type을 지정하지 않아도 컴파일러가 초기화 식을 분석하여 변수의 타입을 지정해준다. (type inference)
	당연한 얘기지만 초기화 식을 사용하지 않고 변수를 선언하려면 변수의 타입을 반드시 명시해야 한다. 초기화 식이 없다면 컴파일러가 타입을 추론할 수 없기 때문이다.

#### 2.2. 코틀린의 변수와 상수
	코틀린에서 변수 선언시 사용하는 키워드는 다음과 같은 2가지가 있다.
	**val(값을 뜻하는 value에서 따옴)** - 변경 불가능한(immutable) 참조를 저장하는 변수다. 자바의 final변수에 해당한다.
	**var(변수를 뜻하는 variable에서 따옴)** - 변경 가능한(mutable) 참조다. 자바의 일반 변수에 해당한다.
	
#### 2.3. 문자열 템플릿 (string template)
코틀린에서는 문자열 리터럴의 필요한 곳에 변수를 넣되 변수 앞에 $를 추가하여 변수를 문자열안에서 사용할 수 있다.

```kotlin
fun main(args: Array<String>) {
	if (args.size > 0) {
        println("Hello, ${args[0]} !")
	}
}
```

#### 2.4. 프로퍼티
```kotlin
	class Person {
		val name: String,
		val isMarried: Boolean
	}
```
코틀린에서의 프로퍼티는 위의 예제와 같이 선언할 수 있다.
val로 선언한 프로퍼티는 읽기전용이며, var로 선언한 프로퍼티는 변경 가능하다.
읽기 전용 프로퍼티의 경우 getter가 생성되고 변경할 수 있는 프로퍼티의 경우 getter와 setter가 선언된다.
코틀린은 이러한 디폴트 접근자 구현을 제공한다.

**Person 클래스를 코틀린에서 사용하기**
```kotlin
>>> val person = Person("minsu", true)
>>> println(person.name)
minsu
>>>println(person.isMarried)
true
```
getter를 호출하는 대신 프로퍼티를 직접 사용했음에 유의하라.
변경 가능한 프로퍼티의 setter도 마찬가지 방식으로 동작한다. 자바에서는 person.setMarried(false)로 사용하지만, 코틀린에서는 person.isMarried = false로 사용한다.
> backing field (뒷받침하는 필드)
대부분의 프로퍼티에는 그 프로퍼티의 값을 저장히기 위한 필드가 있다. 이를 backing field라고 부른다.


## 3. 스마트 캐스트
**개념:** 프로그래머 대신 컴파일러가 캐스팅을 해줌.
**주의사항:** 스마트 캐스트는 is로 변수에 든 값의 타입을 검사한 다음 그 값이 바뀔 수 없는 경우에만 작동한다. 예를 들어 클래스의 프로퍼티에 대해 스마트 캐스트를 사용한다면 그 프로퍼티는 반드시 val이어야 하며 커스텀 접근자를 사용하지 않아야 한다. val이 아니거나 val이지만 커스텀 접근자를 사용하는 경우에는 해당 프로퍼티에 대한 접근이 항상 같은 값을 내놓는다고 확신할 수 없기 때문이다.

## 4. 코틀린의 예외처리
```kotlin
fun readNumber (reader: BufferedReader) : Int? {
	try {
		val line = reader.readLine()
		return Integer.parseInt(line)
	}
	catch (e: NumberFormatException) {
		return null
	}
	finally {
		reader.close()
	}
}
```
코틀린과 자바의 예외처리 코드 중 가장 큰 차이는 throws 절이 코드에 없다는 점이다.
자바에서는 함수를 작성할 때 해당 함수가 던질 가능성이 있는 예외나 그 함수가 호출한 다른 함수에서 발생할 수 있는 예외를 모두 catch로 처리해야 하며, 처리하지 않은 예외는 throws 절에 명시해야 한다.
하지만 코틀린은 다른 최신 JVM언어와 마찬가지로 checked, unchecked exception를 구별하지 않으며 함수가 던지는 예외를 지정하지 않고 발생한 예외를 잡아내도 되고 잡아내지 않아도 된다.

코틀린은 실제 자바 프로그래머들이 checked exception 를 사용하는 방식을 고려하여 설계되었다.
자바는 checked exception 처리를 강제한다. 하지만 프로그래머들이 의미없이 예외를 던지거나 예외를 잡되 처리하지는 않고 그냥 무시하는 코드를 작성하는 경우가 흔하다. 그로 인해 예외처리 규칙이 실제로는 오류 발생을 방지하지 못하는 경우가 자주있다.