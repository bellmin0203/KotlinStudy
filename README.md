# KotlinStudy
책 '코틀린으로 쇼핑몰 앱 만들기' 공부

# chapter 01 코틀린 속성 입문
- 컴파일러가 a + b에서 타입을 추론할 수 있으므로 반환값을 명시하지 않아도 된다.
```
fun sum(a: Int, b: Int) = a + b
```
---
- 타입을 생략할 수 있지만 값을 초기화하지 않는 경우 다음과 같이 타입을 명시해주어야 한다.
``` 
val a: Int
a = 1
```
---
- 코틀린에서는 기본적으로 불변 변수인 val로 선언하고 필요한 경우에만 var를 사용하는 것이 바람직하다고 여겨진다.
- val로 선언했을지라도 그 참조가 가리키는 객체의 내부 값은 변경될 수 있다.
---
- 코틀린의 if-else문은 각 분기의 마지막 표현식의 결과값을 반환한다.
```
val dollar = 4
val class = if(dollar >= 4) {
    "부자"
} else {
    "안부자"
}
```
```
val dollar = 4
val class = if(dollar > 4) "부자" else "안부자"
```
---
## when문
- when에 아무 인자도 주어지지 않는 경우 if-else if 문을 대체해 훨씬 깔끔한 코드를 만들 수도 있다.
```
val x = 2

when {
  x.isOdd() -> print("x is odd")
  x.isEven() -> print("x is even")
  else -> print("x is funny")
}
```
---
- when 또한 값을 반환한다.
```
val x = (1..10).random()
val y = when {
    x in 1..5 -> x * 2
    x in 6..10 -> x + 100
    else -> 0
}
```
---
## 예외
- 코틀린에는 확인된 예외(Checked Exception)가 존재하지 않는다.
- 코틀린의 throw 구문은 표현식이기 때문에 객체의 값이 null인 경우 예외를 던지는 코드[엘비스 연산자(?:)]를 다음과 같이 사용할 수 있다.
```
val s = person.name ?: throw IllegalArgumentException("Name required")
```
---
## 널 안정성
- 코틀린의 타입 시스템은 널(null)값을 가진 객체의 위험을 제거하기 위해 설계되었다.
- 널값을 가질 수 있는 변수의 자체 메서드에 접근할 수 있는 방법은 다음과 같이 제공된다.
```
1. if문으로 널체크
    val b: String? = makeSomeString()
    val l = if(b != null) b.length else -1

2. 세이프콜(Safe call)
    val b: String? = makeSomeString()
    val l = b?.length // 이 경우 l의 타입은 널을 가질 수 있는 Int?가 됨

3. 엘비스 연산자
    val b: String? = makeSomeString()
    val l = b?.length ?: -1 // l의 타입은 Int
```
- !! 연산자는 널값을 가질 수 있는 타입의 객체를 강제로 널값을 가질 수 없는 타입으로 변환해준다. 만약 그 객체의 값이 널이라면 NullPointerException 발생
---
## 범위 함수
- 특정 코드 블록을 한 객체의 범위 내에서 실행하고 싶을 때 사용할 수 있는 함수
```
Person("Alice", 20, "Amsterdam").let {
    println(it)
    it.moveTo("London")
    it.incrementAge()
    println(it)
}
```
```
val alice = Person("Alice", 20, "Amsterdam")
println(alice)
alice.moveTo("London")
alice.incrementAge()
println(alice)
```
- 범위 함수는 특별한 기능은 없지만 코드 가독성을 향상시킬 수 있다.
- 범위 함수의 종류
    * let : 컨텍스트 객체는 it이 되며 마지막 표현식의 결과를 반환, it 대신 명시적인 변수명을 사용할 수 있음
    * apply : 컨텍스트 객체는 this가 되며 컨텍스트 객체 자신을 반환
    * run : 컨텍스트 객체는 this가 되며 마지막 표현식의 결과를 반환
    * also : 컨텍스트 객체는 it이 되며 컨텍스트 객체 자신을 반환
    * with : 컨텍스트 객체는 this가 되며 마지막 표현식의 결과를 반환, 함수의 인자로 객체가 필요하다는 점에서 run과 다름

- 다음은 공식 문서에 적혀있는 범위 함수들 중 어느 것을 선택할지에 대한 짧은 가이드
    * 널이 아닌 객체에 대해 코드 블록 실행 : let
    * 로컬 범위에서 변수로서의 표현식 실행 : let
    * 객체 초기화 : apply
    * 객체를 초기화하면서 결과값을 계산 : run
    * 표현식이 필요한 실행문 : run
    * 부수적인 효과 : also
    * 객체의 함수 호출을 그룹핑 : with
---
## 클래스와 프로퍼티
- 클래스 내부에 선언한 변수를 자바에서는 멤버변수라고 부르는 것과 달리 프로퍼티(property)라고 부른다.
- 프로퍼티는 실제 값을 가지는 필드와 get, set 등의 접근자 함수가 기본적으로 제공된다. (커스텀 가능)
```
class Adder {
    // a와 b는 기본 제공되는 get, set 모두 사용 가능
    var a: Int = 0
    var b: Int = 0
    
    // val로 선언하면 읽기 전용 프로퍼티가 되기 때문에 커스텀 get만 구현
    val sum: Int
        get() {
            return a + b
        }
}
```
```
val adder = Adder() // new 키워드가 필요하지 않음
adder.a = 1 // a의 set 접근자 호출
adder.b = 1 // b의 set 접근자 호출
println(adder.sum) // sum의 get 접근자 호출
```

- 클래스에 함수가 없이 값만 필요한 경우 중괄호를 생략 가능, 아래와 같이 값만 가지는 클래스를 value object라고 부른다.
- 코틀린에서는 value object를 위한 특별한 한정 키워드인 __data class__가 존재한다.
```
class Person(val name: String)
```
---
## companion object
- 코틀린에서는 자바와 달리 static 키워드가 존재하지 않는다. 대신 companion object {...} 블록 안에 정적 멤버들을 추가할 수 있다.
```
class WithStaticMember {
    companion object {
        val hello = "world"
    }
}

println(WithStaticMember.hello)
```
---
## 싱글톤
```
object SingletonClass {
    // ...
}
```
- object로 선언한 클래스는 선언과 동시에 객체가 생성된다. object로 선언된 클래스는 val singleton = SingletonClass()와 같이 다시 객체화할 수 없다.
---
## 고차함수와 람다
- 코틀린의 함수는 일반 객체와 같이 취급 가능하다. 변수에 할당될 수 있고 자료구조에 사용될 수 있으며, 다른 함수의 매개변수로 전달되거나 반환값으로 사용될 수도 있다.
```
fun runTransformation(f: (String, Int) -> String): String {
    return f("hello", 3)
}
```
- 매개변수로 선언된 f는 함수타입으로 정의되어 있다. (String, Int) -> String이 바로 함수 타입이다.
- 이 함수타입의 인스턴스를 얻기 위해서는 다음과 같은 몇가지 방법을 사용할 수 있다.
```
## 람다
{ str, num -> str + num.toString() }
```
```
## 익명함수
fun(str: String, num: Int): String {
    return str + num.toString()
}
```
```
## 함수 참조
String::toInt
```
```
## 함수 타입 인터페이스를 구현한 클래스의 인터페이스
class SomeTransformer: (String, Int) -> String {
    override operator fun invoke(str: String, num: Int): String = TODO()
}

val someFunction: (String, Int) -> String = SomeTransformer()
```
- 코틀린에서는 함수의 매개변수가 함수 하나이거나 마지막 매개변수가 함수일 경우 괄호 바깥에, 혹은 괄호 없이 람다를 넘겨줄 수 있다.
```
fun higherOrderFunction(stringParam: String, funParam: () -> String) {
    println(stringParam + funParam())
}

fun main() {
    higherOrderFunction("Hello ") {
        "World"
    }
}
```
- 만약 매개변수가 함수 하나만 있다면 다음과 같이 괄호도 생략 가능
```
higherOrderFunction {
    "Hello World"
}
```
---
## 확장 함수와 리시버
- 클래스 상속이나 데코레이터 패턴 등을 이용하지 않고 클래스의 기능을 확장할 수 있는 방법
```
val intToLong: (Int) -> Long = { toLong() }
```
- 여기에서 toLong()은 존재하지 않는 함수이기 때문에 컴파일 오류가 발생
- 이 함수는 Int가 유일한 매개변수이기 때문에 람다 블록 내에서 it 키워드로 Int 매개변수로 접근할 수 있다.
```
val intToLong: (Int) -> Long = { it.toLong() }
```
- 하지만 람다 블록이 중첩된다면 it이 중복됨으로 인해 문제가 발생한다. 그리고 경우에 따라서는 it이라는 키워드가 너무 많아지게 되는 문제도 있다.
- 여기에서 바로 리시버가 등장한다. 이 코드 블록을 Int 타입을 리시버로 가지는 함수타입에 할당함으로써 it 없이 동작하게 만들 수 있다.
```
val intToLong: Int.() -> Long = { toLong() }
```
- 확장 함수의 블록 내부에서는 확장하려고 하는 클래스가 리시버 타입이 된다.
```
fun MutableList<T>.swap(index1: Int, index2: Int) {
    val tmp = this[index1] // 'this'는 MutableList의 인스턴스
    this[index1] = this[index2]
    this[index2] = tmp
}
```
---
## 코틀린 DSL
- DSL은 특정 영역에 초점을 맞춘 언어이다. 우리가 쉽게 접할 수 있는 DSL로는 SQL이 있다.
- 코틀린과 같은 GPL(General-purpose Language)을 이용해 DSL을 만들어낸 것을 internal-DSL이라고 부른다. 어떤 독립적인 문법을 만들어내는 것이 아니라 기존 언어의 문법을 특별한 방식으로 사용할 수 있도록 만들어주는 것이다.

```
class Person {
    var name: String? = null
    var age: Int? = null
    var address: Address? = null
}

class Address {
    var mandatory: String? = null
    var detail: String? = null
    var postalCode: String? = null
}
```
만들어볼 DSL의 최종 모습은 다음과 같다.
```
val person: Person = person {
    name = "김스타"
    age = 29
    address {
        mandatory = "서울시 강남구 홍길동 123"
        detail = "707호"
        postalCode == "06076"
    }
}
```
1. 먼저 Person 객체를 초기화해주는 person 함수를 선언한다.
```
fun person(init: (Person) -> Unit): Person {
    val person = Person()
    init(person)
    return person
}

val person: Person = person {
    it.name = "김스타"
    it.age = 29
}
```
- 매번 it을 붙여줘야 하는 불편함이 있다. 이를 해결하기 위해서는 init 함수의 리시버 타입을 Person 클래스로 지정해주면 해결가능하다.
```
fun person(init: Person.() -> Unit): Person {
    val person = Person()
    init(person)
    return person
}
```
2. 내부의 address를 Person의 멤버로서 초기화해주기 위해서는 Person이 address 함수를 가지고 있어야 한다. Person의 확장함수로 이를 해결한다.
```
fun Person.address(init: Address.() -> Unit) {
    val address = Address()
    init(address)
    this.address = address
}
```
- 이 DSL에서처럼 람다 내부에 또다른 람다가 있을 때, 안쪽의 람다는 바깥쪽 람다의 리시버에 접근할 수 있다. address {...} 블록 안에서 Person의 name을 수정할 수가 있다. 이 경우에 원하지 않거나 예상하지 못한 버그가 발생할 수 있다. 이를 해결하기 위해서는 @DslMarker 애노테이션을 이용한다.
```
@DslMarker
annotation class PersonDsl

@PersonDsl
class Person {
    var name: String? = null
    var age: Int? = null
    var address: Address? = null
}

@PersonDsl
class Address {
    var mandatory: String? = null
    var detail: String? = null
    var postalCode: String? = null
}
```
- 이제 address {...} 블록 내부에서는 Person의 멤버들을 수정할 수가 없다.
---
## 문자열 템플릿
```
val a = "Kotlin"
println("Hello, $a")
```
- 중괄호를 이용하면 다음과 같이 짧은 표현식을 넣는 것도 가능하다.
```
val a = "Kotlin"
println("Length = ${a.length}") // Length = 6

val b = (1..10).random()
println("b ${if(b <= 5) "<= 5" else "> 5"}") // b <= 5 혹은 b > 5
```
---
## 코루틴(Coroutine)
- 코틀린 버전 1.3부터 정식으로 채택된 비동기 라이브러리이며 경량 스레드(light-weight threads)라고 생각할 수 있다.
- OS에 의존적인 스레드와 달리 스레드간 컨텍스트 전환 비용이 발생하지 않으며 개발자가 직접 중지 지점을 선택할 수 있다.
- 코루틴을 사용하면 기존의 복잡했던 비동기 로직을 아주 단순하게 구현할 수 있다.
```
import kotlinx.coroutines.*

fun main() {
    GlobalScope.launch {
        delay(1000L)
        println("World!")
    }
    println("Hello,")
    Thread.sleep(2000L)
}
```
이 코드의 출력 결과
```
Hello,
World!
```    
- 코루틴은 특정 코투린 스코프 켄텍스트의 launch 빌더를 통해 실행될 수 있으며 이 빌더 블록 안의 코드는 스레드처럼 비동기로 실행된다.

1. 코루틴을 사용하기 위해 build.gradle에 의존성 추가
```
implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:$coroutine_version"
```
- 코루틴의 기본적인 사용 방법
```
val scope = CoroutineScope(Dispatchers.Default)
scope.launch {
    // do something
}
```
- launch 빌더는 다음과 같이 어느 스케줄러에서 비동기 로직이 실행될지를 지정할 수 있다.
```
scope.launch(Dispatchers.IO) {
    // some I/O
}
```
- 서스펜딩(suspending) 함수 : 현재 코루틴이 실행 중인 스레드를 블록킹하지 않으면서 실행 중인 코루틴을 잠시 중단시킬 수 있는 중단 지점 함수
- 서스펜딩 함수를 호출하는 시점에 현재 실행 중인 코루틴은 잠시 중단되며 그 때 남게 되는 스레드는 다른 코루틴에 할당할 수 있다.
- 서스펜딩 함수의 로직이 끝났을 때에 중단되었던 코루틴은 다시 실행될 준비가 된다.
```
suspend fun someAsyncFunction() {
    // do something
}
```
- 네트워크 호출을 통해 UI를 업데이트해주는 경우
```
suspend fun someNetworkCall(): String {
    delay(1000)
    return "data from network"
}

suspend fun uiFunction() {
    val data = someNetworkCall()
    println(data)
    println("uiFunction is done.")
}
```
- uiFunction()을 호출하게 되면 내부적으로 someNetworkCall()의 호출이 일어나게 되고 uiFunction() 함수는 someNetworkCall()의 호출이 끝날 때까지 중단되게 된다. 하지만 uiFunction()을 실행하고 있던 스레드는 여기서 대기하는 것이 아니라 다른 코루틴에 할당될 수 있는 상태가 된다.
```
suspend fun someNetworkCall(): String {
    delay(1000)
    return "data from network"
}

suspend fun uiFunction() {
    println("I'm uiFunction. ${Thread.currentThread().name}")
    val data = someNetworkCall()
    println(data)
    println("uiFunction is done.")
}

fun main() {
    val scope = CoroutineScope(Dispatchers.Default)

    scope.launch {
        println("I'm coroutine. ${Thread.currentThread().name}")
        uiFunction()
    }
    
    Thread.sleep(500)
    
    scope.launch {
        println("I'm another coroutine. ${Thread.currentThread().name}")
    }
    
    println("main is done. ${Thread.currentThread().name}")
    Thread.sleep(2000)
}
```
- 실행 결과 (JVM 옵션에 -Dkotlinx.coroutines.debug)
```
I'm coroutine. DefaultDispatcher-worker-1 @coroutine#1
I'm uiFunction. DefaultDispatcher-worker-1 @coroutine#1
main is done. main
I'm another coroutine. DefaultDispatcher-worker-1 @coroutine#2
data from network
uiFunction is done.
```
- 서스펜딩 함수를 사용할 때에 주의할 점은 서스펜딩 함수를 코루틴 내부나 또다른 서스펜딩 함수 내부에서만 호출할 수 있다는 것이다.
### 자주 사용되는 코루틴 빌더들과 그 특성
- runBlocking : 현재 스레드를 블록킹하는 코루틴 빌더, 일반 함수에서 서스펜딩 함수를 호출하기 위해 사용할 수 있는 가장 단순한 형태의 코루틴 빌더로 내부의 서스펜딩 함수들도 모두 현재 스레드를 블록킹하게 된다. 주로 메인 함수에서 탑레벨로 사용된다.
- launch : 현재 스레드를 블록킹하지 않고 새로운 비동기 작업을 시작한다. 결과를 받을 수 없기때문에 파이어 앤드 포겟(Fire-and-forget)방식의 유즈케이스에 많이 사용된다. 예외가 전파되지 않기 때문에 블록 내부에서 try-catch가 필요할 수 있다.
- async : 현재 스레드를 블록킹하지 않고 새로운 비동기 작업을 시작한다. Deffered 타입의 객체를 반환하며 await()을 호출해 결과값을 반환받을 수 있다. await()은 서스펜딩 함수이기 때문에 코루틴 내부나 또다른 서스펜딩 함수 내부에서 호출되어야 한다. 예외가 전파되기 때문에 블록 밖에서 try-catch로 예외 처리가 가능하다.
