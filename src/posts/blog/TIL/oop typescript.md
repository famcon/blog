---
title: "OOP typescript"
category: "TIL"
date: "2022-04-01"
desc: "oop with typescript"
thumbnail: "../images/oopts.png"
alt: "alt"
---

# ❓객체지향 프로그래밍이란

여러가지 프로그래밍 패러다임 중 하나로 명령과 절차지향 프로그래밍과 대립되는 개념으로 많이 소개됩니다.

> **절차지향 프로그래밍**
> 정의된 순서의 함수가 하나 씩 호출되는 프로그래밍입니다.
> 함수가 얽혀있기 때문에 하나를 수정하면 사이드 이펙트가 발생할 확률이 높고, 그렇기에 유지보수성과 확장성이 떨어집니다.

이와 달리 **OOP**는 서로 관련있는 프로그램을 객체단위로 만들어 나가는 것을 말합니다.

따라서 하나를 수정하더라도 **사이드 이펙트**가 발생할 확률이 줄어들며 새로운 기능을 추가하거나 유지보수를 하기가 쉬워집니다.

대게는 **클래스**를 사용해서 **객체**를 만듭니다.

_💡 **클래스**는 어떻게 생겼는 지를 묘사하고 있습니다. 이 클래스에 실제 데이터를 넣어서 **객체**를 만들어냅니다._

<br>

<br>

<br>

# 📌 OOP 4가지 원칙

### 1. 캡슐화

OOP의 특징 중 하나는 흩어져 있는 데이터와 함수들 중 관련도가 높은 것을 캡슐화할 수 있다는 것입니다. 캡슐화를 하면서 외부에는 보일 필요가 없는 것들을 숨길수 가 있으며, 외부에서 알 수 없는 어떤 로직을 수행되도록 할 수 있습니다.

### 2. 추상화

OOP의 또 다른 특징은 내부의 기능을 이해하지 못하고 또는 무엇이 있는 지 알지 못하더라도,
간단한 인터페이스를 통해서 객체를 사용할 수 있다는 것입니다.

### 3. 상속성

클래스를 한 번 정의해두면 상속을 통해 재사용할 수 있습니다. 이렇게 상속 관계를 맺게 되는 클래스는 부모, 자식 또는 수퍼, 서브 클래스로 불리게 됩니다.

### 4. 다형성

마지막으로 상속을 통해서 만들어진 클래스는 부모클래스의 공통된 함수를 호출할 수 있으며 이런한 특성을 다형성이라고 합니다.

<br>

<br>

<br>

# 📌 class 구현

클래스는 `class` 를 사용해서 선언합니다.

class를 선언할 때는 `constructor` 을 생성자를 정의해줘야 합니다.

> class 선언문 안에서 변수와 함수를 정의하는데 TS는 번거러움을 줄여주기 위해 constructor에서 동시에 변수를 선언할 수 있는 기능이 있습니다.
> constuctor로 전달받는 인자 앞에 접근지정자를 추가해주면 자동으로 class의 변수가 생성이 되고 생성자가 호출될 때 알아서 데이터를 할당해줍니다.

```tsx
constructor(private highSpeed : H, private lowSpeed : L){}
```

클래스를 통해 수 많은 객체를 만들 수 있는데 객체가 생성될 때마다 변하지 않는 똑같은 변수가 계속해서 할당이 되면 메모리의 낭비가 발생합니다.
이를 방지하기 위해 그러한 변수 앞에는 `static` 을 사용해서 모든 인스턴스에 전역적으로 사용할 수 있도록 합니다.

> 💡**_객체와 인스턴스_**
> 클래스의 타입으로 선언되었을 때 **객체**라고 부르고, 그 객체가 메모리에 할당되어 실제 사용될 때 **인스턴스**라고 부릅니다.

**static**은 함수에도 적용하여 객체를 만들지 않아도 그 함수를 사용할 수 있도록 해주기도 합니다.
예를들어 우리가 javascript의 `Math`모듈을 사용할 때 우리는 객체를 생성하지 않아도 `Math.abs()` API를 사용할 수 있는것도 **static**이 사용된 경우입니다.

<br>

<br>

<br>

# 📌 get & set

getter와 setter는 인스턴스가 생성될 때 전달되는 값을 조합하여 새로운 데이터를 클래스 내부 변수에 할당할 때 사용합니다.

우리는 클래스를 구현할 때 다음과 같은 문제를 만날 수 있습니다.

> **class A**가 있고 이것을 사용해서 `a`라는 인스턴스를 생성합니다. 그리고 생성자를 호출할 때 1과 2를 전달하였고 이것은 `public val1, val2` 변수에 할당됩니다. 마지막으로 A는 전달받은 1과 2를 합한 3을 `newValue`라는 변수에 할당합니다. 이후 `a.val1 = 5`로 재할당하면 우리는 `newValue`가 val1+val2 = 5+2 = `7` 이 되기를 기대합니다.
> 하지만 결과는 `3`으로 변하지 않습니다.

이때 `get` 과 `set`를 사용합니다.

```tsx
private newValue
get age() : {
	return newValue
}
set age(val1, val2){
	this.newValue = val1+val2
}
```

다음과 같이 get를 사용하면 `a.age = 27` 과 같이 변수처럼 호출할 수 있으며 호출과 동시에 newValue에는 두 값을 더한 값이 할당되고, 그 값을 반환합니다.

<br>

<br>

<br>

# 📌 추상화시키기

정말 필요한 인터페이스만 노출시켜 클래스의 사용도를 높여주는 추상화를 할 수 있습니다.

방법은 2가지가 있습니다.

- 접근제어자
- 인터페이스

### interface란

class가 무엇을 의무적으로 구현해야 할 지 규약하는 규약서로 볼 수 있습니다. 규약하고 하는 것을 {}안에 이름과 타입을 선언하여 정의합니다. 새로 생성된 인스턴스는 이 인터페이스를 타입으로 지정받을 수 있습니다.

<br>

<br>

<br>

# 📌 상속

`extends` 를 사용하여 자식 클래스를 만들 수 있습니다.

자식 클래스에서 생성자를 호출할 때는 꼭 `super()`를 호출해야 합니다. super를 호출할 때는 부모가 생성자의 인자를 전달해주면 됩니다.

만약 부모클래스의 함수를 overwriting하고 싶다면 동일한 함수명을 다시 선언해주면 됩니다. 또한, 부모가 정의한 기능을 유지하면서 overwriting을 한다면 `super.<Function>()`을 사용해서 호출 또는 접근한 뒤 overwriting할 수 있습니다.

```tsx
interface speed {
  speedUp(): number
  speedDown(): number
}

class moving implements speed {
  constructor(private highSpeed: number, private lowSpeed: number) {}
  speedUp(): number {
    console.log(`speed up to ${this.highSpeed}`)
    return this.highSpeed
  }
  speedDown(): number {
    console.log(`speed down to ${this.lowSpeed}`)
    return this.lowSpeed
  }
}

class run extends moving {
  constructor(
    private running: number,
    private walking: number,
    private breaktime: number
  ) {
    super(running, walking)
  }
  speedUp() {
    console.log("you can walk when you want")
    return super.speedUp()
  }
  speedDown() {
    console.log("you can walk when you want")
    return super.speedDown()
  }
  takingBreak(): void {
    console.log(`let's take a break for ${this.breaktime} minutes`)
  }
}

const myRun = new run(8, 3, 10)
myRun.speedUp()
myRun.speedDown()
myRun.takingBreak()
```

### 상속의 문제점

상속이 깊어질 수록 관계가 복잡해집니다. 그리고 부모클래스의 행동을 수정하면 이것을 상속하는 모든 자식클래스에 영향을 끼칠 수 있습니다. 그래서 이런 말이 있다고 합니다.

> 상속대신에 composition을 더 선호하라

<br>

<br>

<br>

# 📌 Composition

컴포지션은 필요한 것을 가져와서 조립하는 것을 말합니다.

상속이 무조건 나쁜 것은 아니지만 너무 과도하게 사용하면 관계가 복잡해질 수 있습니다.

컴포지션을 하는 방법은 부품을 만들고, 부품을 장착시키는 것입니다.

구체적으로는 다음과 같습니다.

부품을 장착시킬 클래스의 생성자에 부품을 받아오기 위한 인자를 작성합니다. 이 인자의 타입은 부품(객체)의 클래스입니다.

이 방법은 **의존성 주입(dependency injenction)**이라고 불립니다.

이때 클래스와 클래스가 커플링 되는 방법은 좋지 않습니다. 우리는 커플링을 피하는 방법으로 composition을 더 유연하게 사용하고 싶습니다.

그러기 위해선 클래스들간의 소통은 인터페이스를 사용해야 합니다.

따라서 의존성을 주입받을 때는 인터페이스를 타입으로 지정합시다.

> ex. **붕어빵기계**의 **낡은 반죽기계**를 **새 반죽기계**로 갈아끼워주고 싶습니다.
> 그렇다면 붕어빵기계의 부품 중 반죽기계의 타입은 낡은 반죽기계라고 정의되어 있는 것 보다 그냥 **반죽기계**라고 정의되어 있어야 교체가 수월해질 것입니다.
> 따라서 _낡은 반죽기계와 새 반죽기계는 모두 **반죽기계**라는 인터페이스를 구현_ 하도록 하며, 붕어빵기계는 반죽기계를 의존성주입을 받는 방법으로 구현하는 것이 좋은 방법이 될 것 같습니다.
