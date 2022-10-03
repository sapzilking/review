
# 5장 람다로 프로그래밍

## 1. 람다와 컬렉션

- 코드에서 중복을 제거하는 것은 프로그래밍 스타일을 개선하는 중요한 방법 중 하나다. 코틀린의 컬렉션 라이브러리에서 제공해주는 기능을 사용하여 이를 해결하는 예제를 알아보자.

```kotlin
data class Person(val name: String, val age: Int)
```

<br>
  
> **예제 1.** 사람의 이름과 나이를 저장하는 Person 클래스의 리스트가 주어질 때 가장 연장자를 찾는 기능

- 컬렉션을 직접 검색하기
```kotlin
fun findTheOldest (people: List<Person>) {
  var maxAge = 0
  var theOldest: Person? = null
  for (person in people) {
    if (person.age > maxAge) {
      maxAge = person.age
      theOldest = person
    }
  }
  println(theOldest)
}
```
```kotlin
>>> val people = listOf(Person("Alice", 29), Person("Bob", 31))
>>> findTheOldest(people)
Person(name=Bob, age=31)
```

- 람다를 사용해 컬렉션 검색하기
```kotlin
>>> val people = listOf(Person("Alice", 29), Person("Bob", 31))
>>> println(people.maxBy { it.age }) // 나이 프로퍼티를 비교해서 값이 가장 큰 원소 찾기
Person(name=Bob, age=31)
```

모든 컬렉션에 대해 maxBy 함수를 호출할 수 있다. maxBy는 가장 큰 원소를 찾기 위해 비교에 사용할 값을 돌려주는 함수를 인자로 받는다. 중괄호로 둘러싸인 코드 `{it.age }`는 바로 비교에 사용할 값을 돌려주는 함수다. 이 코드는 컬렉션의 원소를 인자로 받아서(it이 그 인자를 가리킨다) 비교에 사용할 값을 반환한다. 이 예제에서는 컬렉션의 원소가 Person객체 였으므로 이 함수가 반환하는 값은 Person 객체의 age 필드에 저장된 나이 정보다.
이런 식으로 단지 함수나 프로퍼티를 반환하는 역할을 수행하는 람다는 멤버 참조로 대치할 수 있다.

- 멤버 참조를 사용해 컬렉션 검색하기
```kotlin
people.maxBy(Person::age)
```

> `it` 을 사용하는 관습은 코드를 간단하게 만들어준다. 하지만 이를 남용하면 안된다. 특히 람다 안에 람다가 중첩되는 경우 각 람다의 파라미터를 명시하는 편이 낫다.

---

- 함수 파라미터를 람다 안에서 사용하기
```kotlin
fun printMessageWithPrefix(messages: Collection<String>, prefix: String) {
  messsages.forEach{
    println("$prefix $it")  
  }
}
```
```kotlin
>>> val errors = listOf("403 Forbidden", "404 Not Found")
>>> printMessagesWithPrefix(errors, "Error:")
Error: 403 Forbidden
Error: 404 Not Found
```

코틀린에서는 자바와 달리 람다에서 람다 밖 함수에 있는 final이 아닌 변수에 접근할 수 있고, 그 변수를 변경할 수도 있다. 아래 예제의 `prefix, clientErrors, serverErrors` 와 같이 람다 안에서 사용하는 외부 변수를 `람다가 포획(capture)한 변수` 라고 부른다.
```kotlin
fun printProblemCounts(responses: Collection<String>) {
  var cliendErrors= 0
  var serverErrors = 0
  responses.forEach {
    if (it.startsWith("4")) {
      clientErrors++
    } else if (it.startsWith("5")) {
      serverErrors++
    }
  }
  println("$clientErrors client errors, $serverErrors server errors")
}
```
```kotlin
>>> val responses = listOf("200 OK", "418 I'm a teapot", "500 Internal Server Error")
>>> printProblemCounts(responses)
1 client errors, 1 server errors
```

- 지연 계산(lazy) 컬렉션 연산

```kotlin
people.map(Person::name).filter { it.startsWith("A") }
```
코틀린 표준 라이브러리 참조 문서에는 filter와 map이 리스트를 반환한다고 써 있다.
이는 이 연쇄 호출이 리스트를 2개 만든다는 뜻이다.
원본 리스트에 원소가 2개밖에 없다면 리스트가 2개 더 생겨도 큰 문제가 되지 않겠지만, 원소가 수백만 개가 되면 훨씬 더 효율이 떨어진다.
이를 더 효율적으로 만들기 위해서는 각 연산이 컬렉션을 직접 사용하는 대신 시쿼스를 사용하게 만들어야 한다.
```kotlin
people.asSequence() // 원본 컬렉션을 시퀀스로 변환한다.
  .map(Person::name)                       | 시퀀스도 컬렉션과 똑같은
  .filter { it.startsWith("A") }           | API를 제공한다.
  .toList() // 결과 시퀀스를 다시 리스트로 변환한다.
```
위의 코드는 이전의 예제와 결과가 같다. 하지만 중간 결과를 저장하는 컬렉션이 생기지 않기 때문에 원소가 많은 경우 성능이 눈에 띄게 좋아진다.

- **시퀀스 연산 실행: 중간 연산과 최종 연산**
시퀀스에 대한 연산은 중간연산과 최종 연산으로 나뉜다.  중간 연산은 다른 시퀀스를 반환한다. 

![KakaoTalk_Photo_2022-10-03-20-42-35](https://user-images.githubusercontent.com/93430103/193594814-b8fc8482-5ea4-4d43-8198-8a815e9db9fd.jpeg)

중간 연산은 항상 지연 계산된다. 최종 연산이 없는 예제를 살펴보자.
```kotlin
>>> listOf(1,2,3,4).asSequence()
      .map { print("map($it) "); it * it }
      .filter { print("filter($it) "); it % 2 == 0 }
```
위 코드를 실행하면 아무 내용도 출력되지 않는다. 이는 map과 filter 변환이 늦춰져서 결과를 얻을 필요가 있을 때(즉 최종 연산이 호출될 때) 적용된다는 뜻이다.
```kotlin
>>> listOf(1,2,3,4).asSequence()
      .map { print("map($it) "); it * it }
      .filter { print("filter($it) "); it % 2 == 0 }
      .toList()
map(1) filter(1) map(2) filter(4) map(3) filter(9) map(4) filter(16)
```
이 예제에서 연산 수행 순서를 잘 알아둬야 한다. 직접 연산을 구현한다면 map 함수를 각 원소에 대해 먼저 수행해서 새 시퀀스를 얻고, 그 시퀀스에 대해 다시 filter를 수행할 것이다. 컬렉션에 대한 map과 filter는 그런 방식으로 작동한다. 하지만 시퀀스에 대한 map과 filter는 그렇지 않다. 시퀀스의 경우 모든 연산은 각 원소에 대해 순차적으로 적용된다. 즉 첫 번째 원소가 (변환된 다음에 걸러지면서) 처리되고, 다시 두 번째 원소가 처리되며, 이런 처리가 모든 원소에 대해 적용된다.
따라서 원소에 연산을 차례대로 적용하다가 결과가 얻어지면 그 이후의 원소에 대해서는 변환이 이뤄지지 않을 수도 있다.

```kotlin
>>> val people = listOf(Person("Alice", 29), Person("Bob", 31), Person("Charles", 31), Person("Dan", 21))
>>> println(people.asSequence().map(Person::name)
              .filter { it.length < 4 }.toList()) // map 다음에 filter 수행
[Bob, Dan]
>>> println(people.asSequence().filter { it.name.length < 4 }
              .map(Person::name).toList()) // filter 다음에 map 수행
[Bob, Dan]
```

![KakaoTalk_Photo_2022-10-03-22-54-49](https://user-images.githubusercontent.com/93430103/193595257-8136723e-7548-455c-8f7b-205ed35b5ca6.jpeg)

map을 먼저 하면 모든 원소를 변환한다. 하지만 filter를 먼저 하면 부적절한 원소를 먼저 제외하기 때문에 그런 원소는 변환되지 않는다.


- **수신 객체 지정 람다: with와 apply**
	- with 함수
	```kotlin
	fun alphabet() : String {
	  val result = StringBuilder()
	  for (letter in 'A'...'Z') {
	    result.append(letter)
	  }
	  result.append("\nNow I know the alphabet!")
	  return result.toString()
	}
    >>> println(alphabet())
    ABCDEFGHIJKLMNOPQRSTUVWXYZ
    Now I know the alphabet!
	```
	이 예제에서 result에 대해 다른 여러 메서드를 호출하면서 매번 result를 반복 사용했다. 이 정도 반복은 그리 나쁘지 않지만 이 코드가 훨씬 더 길거나, result를 더 자주 반복해야 했다면 어땠을까?
	이제 앞의 예제를 `with`로 다시 작성한 결과를 보자.
	```kotlin
	fun alphabet() : String {
	  val stringBuilder = StringBuilder()
	  return with(stringBuilder) { // 메서드를 호출하려는 수신 객체를 지정한다.
	    for (letter in 'A'...'Z') {
	      this.append(letter) // "this"를 명시해서 앞에서 지정한 수신객체의 메서드를 호출한다.
	    }
	    append("\nNow I know the alphabet!") //"this"를 생략하고 메서드를 호출한다.
	    this.toString() // 람다에서 값을 반환한다.
	  }
	}
	```
	`with` 문은 언어가 제공하는 특별한 구문처럼 보이지만 실제로는 파라미터가 2개 있는 함수다.
	`with` 함수는 첫 번째 인자로 받은 객체를 두 번째 인자로 받은 람다의 **수신 객체**로 만든다.
	인자로 받은 람다 본문에서는 this를 사용해 그 수신 객체에 접근할 수 있다.

앞의 alphabet 함수를 더 리팩토링 해서 불필요한 stringBuilder 변수를 없앨 수도 있다.
```kotlin
fun alphabet() = with(StringBuilder()) {
  for (letter in 'A'..'Z') {
    append(letter)
  }
  append("\nNow I know the alphabet!")
  toString()
}
```
`with` 가 반환하는 값은 람다 코드를 실행한 결과며, 그 결과는 람다 식의 본문에 있는 마지막 식의 값이다. 하지만 때로는 람다의 결과 대신 수신 객체가 필요한 경우도 있다. 그럴 때는 `apply` 라이브러리 함수를 사용할 수 있다.

- apply 함수
`apply` 함수는 `with` 와 거의 같다. 유일한 차이란 항상 자신에게 전달된 객체(즉 수신 객체)를 반환한다는 점뿐이다. `apply` 를 써서 alphabet 함수를 다시 리팩터링 해보자.

```kotlin
  fun alphabet() = StringBuilder().apply {
    for (letter in 'A'..'Z') {
      append(letter)
    }
    append("\nNow I know the alphabet!")
  }.toString()
```
`apply` 는 확장 함수로 정의돼 있다. `apply` 의 수신 객체가 전달받은 람다의 수신 객체가 된다. 이 함수에서 `apply` 를 실행한 결과는 StringBuilder 객체다. 따라서 그 객체의 toString을 호출해서 String 객체를 얻을 수 있다.
