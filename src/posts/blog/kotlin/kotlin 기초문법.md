---
title: "Kotlin 기초문법"
category: "kotlin"
date: "2022-10-01"
desc: "kotlin basic grammer"
thumbnail: "../images/kotlin.png"
alt: "alt"
---

# 코틀린 기초문법 정리

## 표기법

- 클래스 - 파스칼 표기법

  ex) AuthService : 대문자로 시작

- 함수, 변수 - 카멜 표기법

  ex) functionName : 소문자로 시작

<br>

## 변수

- var : 읽기, 쓰기 가능
- val : 처음 선언에서만 값을 할당

<br>

## 자료형

- nullable
  - `var a :Int? = null`
  - 코틀린은 null point exception을 막기 위해 원천적으로 값이 할당되지 않는 상황을 차단
  - 따라서 값을 할당하지 않을 때는 **nullable**변수로 선언해주어야 함

<br>

#### 자료형 체크

코틀린은 `is`연산자를 통해 자료형의 참 거짓을 얻을 수 있다.

```kotlin
var a = 5
a is Int // true
a is Char // false
```

<br>

## 형변환

코틀린은 오류를 막기위해 **명시적형변환**만이 가능

- toInt()
- toFloat()
- toChar()

<br>

## 배열

처음 선언한 크기에서 변경이 불가능하다.

- 일반 배열
  - `var arr = arrayOf(1,2,3,4,5)`
- 비어있는 배열
  - `var arr = arrayOfNulls<Int>(5)`

<br>

## 함수

```kotlin
fun add(a:Int, b:Int) : Int {
    return a + b
}
```

위 함수를 단일표현식 함수로 더 짧게 표현할 수 있다.

#### 단일표현식 함수

```kotlin
fun add(a:Int, b:Int) = a + b
```

<br>

## for문

- 일반 반복문
  ```kotlin
  for(i in 0..9) {
      print(i)
  }
  ```
- step
  ```kotlin
  for(i in 0..9 step 3){
      print(i)
  }
  ```
- 감소 반복문
  ```kotlin
  for(i in 9 downTo 0){
      print(i)
  }
  ```
  - .. 대신 `downTo`를 붙인다.
  - 뒤에 `step`을 붙이면 단계를 변경할 수 있다.
- labele
  ```kotlin
  for(i in 0..9){
      for(j in 0..9){
          if(i==1 && j==2) break
          return i+j
      }
      if(i==1 && j==2) break
  }
  ```
  위 코드를 코틀린은 lable을 사용해서 간단하게 처리한다.
  ```kotlin
  loop@for(i in 0..9){
      for(j in 0..9){
          if(i==1 && j==2) break@loop
          return i+j
      }
  }
  ```

<br>

# 클래스

### 생성

```kotlin
class Person (var name:String, val birthYear:Int) {
  init {
      println("안녕하세요.")
  }

  constructor(name: String) : this(name, 1997) {
      println("안녕하세요.")
  }

  fun introduce() {
        println("${birthYear}년생 ${name}입니다")
    }
}
```

- `init`
  - 기본생성자
- `constructor`
  - 보조생성자
  - 항상 기본생성자를 호출해줘야 함
  - `this`를 사용해서 호출

### 상속

```kotlin
open class Animal(var name:String, var age:Int, var type:String) {
    fun introduce() {
        println("저는 ${type} ${name}이고, ${age}살 입니다.")
    }
}

class Dog (name:String, age: Int) : Animal(name, age, "개") {
    fun bark(){
        println("멍멍!")
    }
}
```

- `open`을 사용하여 상속할 수 있는 클래스 생성
- 수퍼클래스와 서브클래스는 같은이름의 속성을 가질 수 없음
- 서브클래스를 생성할 때는 반드시 수퍼클래스의 생성자를 호출해야 함

### 오버라이딩

```kotlin
open class Animal(var name:String, var age:Int, var type:String) {
    fun introduce() {
        println("저는 ${type} ${name}이고, ${age}살 입니다.")
    }
}

class Dog (name:String, age: Int) : Animal(name, age, "개") {
    override fun introduce(){
        println("멍멍!")
    }
}
```

- `open`클래스의 함수는 `override`를 통해 재구현할 수 있음

### 추상화

```kotlin
abstract class Animal {
  abstract fun eat()
  fun sniff() {
    println("킁킁")
  }
}

class Rabbit : Animal() {
  override fun eat() {
    println("당근을 먹습니다.")
  }
}
```

- 추상클래스는 자체적으로는 인스턴스를 만들수 없는 클래스
- 추상함수는 구현부가 없는 함수
- 추상함수를 구현하려면 서브클래스가 `override`해야함

### 인터페이스

```kotlin
interface Runner {
    fun run()
}

interface Eater {
    fun eat() {
        println("음식을 먹습니다.")
    }
}

class Dog : Runner, Eater {
    override fun run(){
        println("우다다다 뜁니다.")
    }

  override fun eat() {
      println("허겁지겁 먹습니다.")
  }
}
```

- 코틀린의 인터페이스는 속성, 추상함수, 일반함수를 모두 가질 수 있다.
- 추상함수는 생성자가 있지만, 인터페이스는 생성자를 가질 수 없음
- 자동 간주
  - 구현부가 있으면 `open`함수
  - 구현부가 없으면 `abstract`함

<br>

# 프로젝트

- 기능단위로 **모듈**을 분리
  - 라이브러리 모듈 등
  - .kt뿐 아니라 여러가지 폴더와 파일로 구성됨
  - 물리적인 구조를 담당
- 소스코드의 소속을 지정하는 **패키지**
  - 논리적인 구조를 담당
  - 코드파일 위에 `package {package name}`을 기입해서 소스코드를 패키지로 분리
  - 패키지를 명시하지 않으면 자동으로 `default`패키지로 묶임
  - 같은 패키지 안에서는 변수, 함수, 클래스를 공유할 수 있음
  - 패키지가 다를 경우 `import`를 통해 불러올 수 음

<br>

# 접근제한자

- 패키지 스코프
  - `public` : 어떤 패키지에서도 접근 가능
  - `internal` : 같은 모듈내에서만 접근 가능
  - `private` : 같은 파일내에서만 접근 가능
- 클래스 스코프
  - `public` : 클래스 외부에서 항상 접근 가능
  - `private` : 클래스 내부에서만 접근 가능
  - `protected` : 자신과 서브클래스에서만 접근 가능

<br>

# 고차함수

함수를 클래스에서 만든 **인스턴스처럼** 취급하는 방법.
함수를 인자로 넘겨주거나, 결과값으로 반환받을 수 있는 방법.

```kotlin
fun main(){
    b(::a)
}

fun a (str: String) {
    println("고차함수입니다.")
}

fun b (f : (String) -> Unit) {
    f()
}
```

- 고차함수로 넘길 땐 함수앞에 콜론을 두개 붙여줌
- 인자로 넘길 함수를 굳이 이름까지 붙여 만들 필요가 없는 경우라면 **람다함수**로 지정

### 람다함수

기본 사용법

```kotlin
fun main(){
  val c : (String) -> Unit = { str -> println("람다합수입니다")}
  // val c : (String) -> Unit = { str :String -> println("람다합수입니다")}
  // 과 같지만 자료형을 지정해줬기 때문에 str은 타입추론이 되어 생략해도 됨
  // 또는 반대로 함수 자료형을 기술하지 않고
  // val c = { str : String -> println("람다함수입니다.")}
  // 로 작성할 수도 있음
  b(::c)
}

fun b (f : (String) -> Unit) {
  f()
}
```

여러 구문으로 사용

```kotlin
val c : (Int, Int) -> Int = { a, b ->
var sum = a + b
return sum * 2
}
```

파라미터가 없는 경우

```kotlin
val a : ()->Unit = {println("파라미터 없음")}
```

파라미터가 하나인 경우

```kotlin
val a: (String)->String = { return  "받은 파리미터는 $it 입니다"}
```

<br>

# 스코프함수

- `apply`
  - 인스턴스를 생성한 후 변수에 담기 전 초기화과정을 수행할 때
  - `인스턴스.apply(){}` 스코프안에서는 참조연산자 `.`을 사용할 필요 없이 바로 속성에 접근 가능
- `run`
- `with`
- `also`
- `let`
