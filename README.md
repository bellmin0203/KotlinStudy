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
- 코틀린의 throw 구문은 표현식이기 때문에 객체의 값이 null인 경우 예외를 던지는 코드를 다음과 같이 사용할 수 있다.
```
val s = person.name ?: throw IllegalArgumentException("Name required")
```


