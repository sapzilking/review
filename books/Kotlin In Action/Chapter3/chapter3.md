# 3장 함수 정의와 호출

## 1. default parameter
자바에서는 일부 클래스에서 overloading한 메서드가 너무 많아진다는 문제가 있다.
코틀린에서는 함수 선언에서 파라미터의 디폴트 값을 지정할 수 있으므로 이런 오버로드 중 상당수를 피할 수 있다.
```kotlin
	fun <T> joinToString {
		collection: Collection<T>,
		seperator: String = ", ",
		prefix: String = "",
		postfix: String = ""
	} : String
```

## 2. 메서드를 다른 클래스에 추가: 확장 함수와 확장 프로퍼티
### 2.1 확장함수
**개념:** 어떤 클래스의 멤버 메서드인것처럼 호출할 수 있지만 그 클래스의 밖에 선언된 함수.

```kotlin
fun String.lastChar(): Char = this.get(this.length - 1)
```
확장 함수를 만들려면 추가하려는 함수 이름 앞에 그 함수가 확장할 클래스의 이름을 덧붙이기만 하면 된다.
여기서 클래스 이름을 **receiver type(수신 객체 타입)** 이라 부르며, 확장 함수가 호출되는 대상이 되는 값(객체)를 **receiver object(수신 객체)** 라고 부른다.
위의 예제에서는 String이 receiver type이고 2군데의 this가 receiver object 이다.
```kotlin
>>> println("Kotlin".lastChar())
n
```
이 예제에서는 String이 receiver type이고 "Kotlin"이 receiver object이다.

## 3. 값의 쌍 다루기: 중위 호출과 구조 분해 선언
코틀린에서 맵을 만들려면 mapOf 함수를 사용한다.
```kotlin
val map = mapOf(1 to "one", 7 to "seven", 53 to "fifty_three")
```
여기서 to 라는 단어는 코틀린 키워드가 아닌 **"중위 호출(infix call)"** 이라는 특별한 방식으로 to 라는 메서드를 호출한 것이다.
인자가 하나뿐인 일반메서드나 인자가 하나뿐인 확장 함수에 중위 호출을 사용할 수 있다.
함수를 중위 호출에 사용하게 허용하고 싶으면 infix 변경자를 함수 선언 앞에 추가해야 한다. 

## 4. 3중 따옴표로 묶은 문자열
**개념:** 이스케이프를 피하거나 줄바꿈을 표현할 때 3중 따옴표 문자열을 사용하여 간결하게 표현할 수 있다.

**e.g**
```kotlin
	fun parsePath (path: String) {
		val regex = """(.+)/(.+)\.(.+)""".toRegex()
		val matchResult = regex.matchEntire(path)
		if (matchResult != null) {
			val (directory, filename, extension) = matchResult.destructured
			println("Dir: $directory, name: $filename, ext: $extension")
		}
	}
```
