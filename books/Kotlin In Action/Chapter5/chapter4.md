
# 4장 클래스, 객체, 인터페이스
## 1. 코틀린에서 메서드가 정의된 인터페이스를 사용하는 방법
- kotlin에서는 override 변경자를 꼭 사용해야 한다. override 변경자는 실수로 상위 클래스의 메서드를 오버라이드하는 경우를 방지해 준다. 상위 클래스에 있는 메서드와 시그니처가 같은 메서드를 우연히 하위 클래스에서 선언하는 경우 컴파일이 되지 않는다.
- 상위 타입의 구현을 호출할 때는 자바와 마찬가지로 super를 사용한다. 차이점은 기반타입이 super앞에 오는 자바와 달리 코틀린에서는 꺾쇠 괄호 안에 기반 타입 이름을 지정한다.


## 2. 상속을 제어하는 변경자
- 자바의 클래스와 메서드는 기본적으로 상속에 대해 열려있지만 fragile base class(취약한 기반 클래스) 문제에 의거하여 코틀린의 클래스와 메서드는 기본적으로 `final`이다.
> **fragile base class 문제**
   하위 클래스가 기반 클래스에 대해 가졌던 가정이 기반 클래스를 변경함으로써 깨져버린 경우에 발생하는 문제
   
  - 코틀린에서 어떤 클래스의 상속을 허용하려면 클래스 앞에 open 변경자를 붙인다.
  - `override`를 허용하고 싶은 메서드나 프로퍼티의 앞에도 open 변경자를 붙인다.
  - 기반 클래스나 인터페이스의 멤버를 오버라이드 하는 경우 그 메서드는 기본적으로 열려있기 때문에 금지하려면 오버라이드 하는 메서드 앞에 final을 명시해야 한다.
  - abstract 클래스에는 구현이 없는 추상 멤버가 있기 때문에 하위 클래스에서 그 추상 멤버를 오버라이드해야만 하는게 보통이다. 추상 멤버는 항상 열려있다. 따라서 추상 멤버 앞에 open 변경자를 명시할 필요가 없다.
	  - 추상 클래스에 속했더라도 비 추상 함수는 기본적으로 final이다.
  
  <br>

|변경자|이 변경자가 붙은 멤버는...|설명|
|---|---|---|
|final|오버라이드 할 수 없음|클래스 멤버의 기본 변경자다.|
|open|오버라이드 할 수 있음|반드시 open을 명시해야 오버라이드 할 수 있다.|
|abstract|반드시 오버라이드 해야 함|추상 클래스의 멤버에만 이 변경자를 붙일 수 있다. 추상 멤버에는 구현이 있으며 안된다.|
|override|상위 클래스나 상위 인스턴스의 멤버를 오버라이드 하는 중|오버라이드하는 멤버는 기본적으로 열려있다. 하위 클래스의 오버라이드를 금지하려면 final을 명시해야 한다.|


## 3. 가시성 변경자
|변경자|클래스 멤버|최상위 선언|
|---|---|---|
|public|모든 곳에서 볼 수 있다.|모든 곳에서 볼 수 있다.|
|internal|같은 모듈 안에서만 볼 수 있다.|같은 모듈 안에서만 볼 수 있다.|
|protected|하위 클래스 안에서만 볼 수 있다.|(최상위 선언에 적용할 수 없음)|
|private|같은 클래스 안에서만 볼 수 있다.|같은 파일 안에서만 볼 수 있다.|

- 자바에서는 같은 패키지 안에서 protected멤버에 접근할 수 있지만 코틀린에서는 그렇지 않다.
  코틀린의 protected 멤버는 오직 어떤 클래스나 그 클래스를 상속한 클래스 안에서만 보인다.


## 4. 주 생성자
> **주 생성자**
- 정의: 클래스 이름 뒤에 오는 괄호로 둘러싸인 코드를 `주 생성자(primary constructor)` 라고 부른다.
```kotlin
class User constructor(_nickname: String) {
  val nickname: String
  init {
    nickname = _nickname
  }
}
---
class User (_nickname: String) {
  val nickname = _nickname
}
---
class User (val nickname: String,
            val isSubscribed: Boolean = true)
```
위의 코드 3가지는 다 같은 역할을 하며, 코틀린에서는 parameter의 default value를 정해줄 수 있다.

- 자바에서는 extends, implements 키워드로 클래스와 인터페이스를 구분하지만
코틀린에서는 클래스와 인터페이스 둘 다 `:` 를 사용하므로 이름 뒤에 괄호가 붙었는지로 이를 구별한다.

### 인터페이스에 선언된 프로퍼티 구현
```kotlin
interface User {
  val nickname: String
}

// 주 생성자에 있는 프로퍼티
class PrivateUser(override val nickname: String) : User

// 커스텀 게터 사용
class SubscribingUser(val email: String) : User {
  override val nickname: String
    get() = email.substringBefore('@') // 커스텀 게터
}

// 프로퍼티 초기화 식 사용
class FacebookUser(val accountId: Int) : User {
  override val nickname = getFacebookName(accountId) // 프로퍼티 초기화 식
}
```
**result**
```
>>> println(PrivateUser("test@kotlinlang.org").nickname)
test@kotlinlang.org
>>> println(SubscribingUser("test@kotlinlang.org").nickname)
test
```

## 5. getter와 setter에서 뒷받침하는 field에 접근
> **setter에서 뒷받침하는 field 접근하기**

```kotlin
class User(val name: String) {
  var address: String = "unspecified"
    set(value: String) {
      println("""Address was changed for $name: "$field" -> "$value".""".trimIndent())
      field = value
    }
}
```
**result**
```
>>> val user = User("Alice")
>>> user.address = "Elsenheimerstrasse 47, 80687 Muenchen"
Address was changed for Alice: "unspecified" -> "Elsenheimerstrasse 47, 80687 Muenchen".
```

접근자의 본문에서는 field라는 특별한 식별자를 통해 뒷받침하는 필드에 접근할 수 있다. getter에서는 filed값을 읽을 수만 있고 setter에서는 field 값을 읽거나 쓸 수 있다.


> `Java`에서 `==`를 primitive type에서는 두 피연산자의 값이 같은지 비교하고(동등성(equality))
reference type에서는 두 피연산자의 주소가 같은지를 비교한다.(참조비교(reference comparision))
`Kotlin`에서는 `==`연산자가 두 객체를 비교하는 기본적인 방법이다. `==`는 내부적으로 equals를 호출해서 객체를 비교한다. 참조 비교를 위해서는 `===`연산자를 사용할 수 있다.

## 6. data 클래스
```kotlin
data class Client (val name: String, val postalCode: Int)
```
data라는 변경자가 붙은 클래스를 데이터 클래스 라고 부른다.
위의 Client 클래스는 자바에서 요구하는 모든 메서드를 포함한다.
- 인스턴스 간 비교를 위한 equals
- HashMap과 같은 해시 기반 컨테이너에서 키로 사용할 수 있는 hashCode
- 클래스의 각 필드를 선언 순서대로 표시하는 문자열 표현을 만들어 주는 toString

## 7. Object 키워드: 클래스 선언과 인스턴스 생성
- object 키워드를 사용하는 상황
	- 객체 선언(object declaration)은 싱글턴을 정의하는 방법 중 하나다.
	- 동반 객체(companion object)는 인스턴스 메서드는 아니지만 어떤 클래스와 관련 있는 메서드와 팩토리 메서드를 담을 때 쓰인다. 동반 객체 메서드에 접근할 때는 동반 객체가 포함된 클래스의 이름을 사용할 수 있다.
	- 객체 식은 자바의 무명 내부 클래스(anonymous inner class) 대신 쓰인다.

### 동반 객체: 팩토리 메서드와 정적 멤버가 들어갈 장소
Kotlin 언어는 Java의 static 키워드를 지원하지 않는다. 그 대신 패키지 수준의 최상위 함수와 객체 선언를 활용한다.

예제를 통해 부 생성자가 2개 있는 클래스를 살펴보고, 다시 그 클래스를 동반 객체안에서 팩토리 클래스를 정의하는 방식으로 변경해보자.

- 부 생성자가 여럿 있는 클래스 정의하기
```kotlin
class User {
  val nickname: String
  
  constructor(email: String) { // 부 생성자
    nickname = email.substringBefore('@')
  }

  constructor(facebookAccountId: Int) { // 부 생성자
    nickname = getFacebookName(facebookAccountId)
  }
}
```

- 부 생성자를 팩토리 메서드로 대신하기
```kotlin
class User private constructor(val nickname: String) { // 주 생성자 비공개
  companion object { // 동반 객체 선언
    fun newSubscribingUser(email: String) = User(email.substringBefore('@'))
    fun new FacebookUser(accountId: Int) = User(getFacebookName(accountId))
  }
}
```
```
>>> val subscribingUser = User.newSubscribingUser("bob@gmail.com")
>>> val facebookUser = User.newFacebookUser(4)
>>> println(subscribingUser.nickname)
bob
```
