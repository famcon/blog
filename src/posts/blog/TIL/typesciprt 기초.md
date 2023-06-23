---
title: "Typescript 기초"
category: "TIL"
date: "2022-05-01"
desc: "typesciprt 기초 문법"
thumbnail: "../images/typescript.jpeg"
alt: "alt"
---

# 🔎 기본타입

- **원시 타입**
  - `number`, `string`, `boolean` ...
- **object**
  - `funtion`, `array` ...
- **기타 타입**

  - `undefined`
    이 타입을 가지는 변수는 undefined만 가져야 합니다. 따라서 의존적 타입으로 볼 수 있습니다.
    의존적이라는 것은 다른 타입과 함께 정의되는 것입니다. 함께 정의하는법은 뒤에서 다시 설명하겠습니다.

  - `null`
    undefined의 특성과 같습니다.

  - `unknown`, `any`
    어떠한 데이터도 담을 수 있습니다.

  - `void`
    아무것도 return 하지 않음을 정의합니다.

---

> **?과 undefined의 차이** > `?`을 앞에 붙은 변수는 정의한 타입을 가지거나 값을 지정해주지 않으면 자동으로 undefined를 가질 수 있습니다.
> 반면 `undefined|number`로 타입을 정의한 변수는 number루 주거나 그렇지 않으면 꼭 undefined를 전달해줘야 합니다.

```tsx
function getnum(a: number, b?: number): void {
  console.log(a)
}
getnum(1)

/// 출력 : 1
```

---

> 💡 **_Rest Parameter_**
> 함수가 전달받는 인자의 개수를 유동적으로 받고싶을 때 `...arr` 과 같이 설정하는데 이때 타입은 만약 number라면 `number[]` 라고 지정할 수 있습니다

```tsx
function getnum(...a: number[]): void {
  let sum: number = 0
  a.forEach((p: number) => (sum += p))
  console.log(sum)
}
getnum(1, 2, 3, 4)
/// 출력 : 10
```

<br>

<br>

# 🔎 Type alias

타입 앨리어스란 새로운 타입을 정의하는 방법입니다.
인터페이스 형태, 원시타입, 유니온타입, 튜플 등을 지정해줄 수 있습니다.

```tsx
type student = {
  name: string
  age: number
  address: string
}
```

---

### 유니온 타입

OR의 특성을 가집니다.
`direction`이라는 타입이 `left | right | up | down`을 가지는 것을 예로 들 수 있습니다.

인터페이스와 유니온을 함께 사용할 수 있습니다.

```tsx
type car = {
  wheel_drive: 2 | 4
  seat: number
  name: string
}

type bike = {
  manufacturer: string
  bascket: boolean
}

type vehicle = car | bike // <= 유니온 타입

const myVehicle: vehicle = {
  manufacturer: "삼천리",
  bascket: false,
}
```

---

### discriminated 유니온

`LoginState` 는 `LoginSuccess` 타입과 `LoginFail` 타입을 가지는 타입이라고 가정합니다.

LoginState의 둘 중 하나의 결과가 나왔을 때 결과에 따른 반응을 하기 위해서 결과에 response가 있는 지를 통해 어떤 결과인지를 판단하고 반응을 구현할 수 있습니다.

이 방법은 object의 키가 많아질 수록 비효율적인 알고리즘이 됩니다.

따라서 success와 fail이 동일하게 가지는 무언가를 넣어주고 그것을 통해 둘을 구분하는 **discriminate** 방법을 사용해야 합니다.

가령, success와 fail은 모두 `result`를 공통적으로 포함하면 LoginState의 결과에서 result가 무엇인지에 따라 그에 맞는 반응을 구현해줄 수 있습니다.

```tsx
type LoginSuccess = {
  result: "success"
  redirect: "move to main page"
}

type LoginFail = {
  result: "fail"
  errorMsg: "check your email or password"
}

type LoginState = LoginSuccess | LoginFail

function redirect(login: LoginState): void {
  if (login.result === "success") {
    console.log(login.redirect)
  } else {
    console.error(login.errorMsg)
  }
}
```

---

### intersection

&를 사용하여 두 개의 타입을 모두 가지는 타입을 설정하는 방법입니다.

---

### enum

enum 키워드 다음에 변수와 함께 {}안에 원소들을 삽입하면 자동으로 0부터 n-1까지가 할당된 타입이 생성됩니다.

하지만 타입스크립트에서는 enum으로 타입이 저징된 변수에 다른 어떤 숫자도 할당이 가능해서 안전하다고 보지 않습니다.

이 경우 아무런 경고메세지가 나오지않아서 위험합니다.

따라서 유니온타입으로 대체하여 사용하는 것이 더 안전하다고 권고합니다.

> 💡 **_타입추론_** > _타입을 선언하지 않고 변수를 정의하면서 어떤 값을 할당하면 할당된 값의 type을 변수 type으로 정의하게 됩니다.
> 하지만 프로젝트에서 코드가 복잡해지면 타입을 정확하게 명시해줌으로써 오류를 줄여나갈 수 있습니다._

<br>

<br>

# 🔎 타입 assertion

타입을 강요할 때 사용합니다.

가급적 사용하지 않는 것이 좋지만 불가피하게 써야하는 경우가 있습니다.

무조건 문자열을 반환하는 것을 알고있으며 반환값의 길이를 사용하고 싶은데 타입이 없기 때문에 `Array.length()` 메서드를 사용하지 못합니다.

이때 뒤에 `as string`을 붙여주면 됩니다. 또는 앞에 `<string>`을 붙여줄 수도 있습니다.

또 타입을 강요하지 않고 진행을 강요하는 `<변수명>!.API` 방법도 있습니다.

<br>

<br>

# 🔎 제네릭

제네릭이란 작성해둔 코드가 사용될 때, 그때서야 명확한 타입이 정의되는 방법입니다.

---

### 클래스의 제네릭

`moving`이라는 클래스는 `speed` 규약을 만족시키는 형태로 구현된다고 가정하겠습니다.
`speed`는 속도를 올릴거나 내리는 함수를 가지도록 규약합니다.
두 개의 함수는 속도를 반환하는 함수입니다.
`moving`은 규약이 받기 원하는 제네릭을 똑같이 가져야 합니다.
생성자에서 제네릭을 받아서 규약과 타입을 만족시키는 함수를 생성합니다.
코드는 다음과 같습니다.

```tsx
interface speed<L, H> {
  speedUp: () => H
  speedDown: () => L
}

class moving<L, H> implements speed<L, H> {
  constructor(private highSpeed: H, private lowSpeed: L) {}
  speedUp(): H {
    return this.highSpeed
  }
  speedDown(): L {
    return this.lowSpeed
  }
}

const running = new moving(15, 11)

let runningSpeed

runningSpeed = running.speedUp()
console.log(`now speed is ${runningSpeed}`)
// "now speed is 빛의속도"

runningSpeed = running.speedDown()
console.log(`now speed is ${runningSpeed}`)
// "now speed is 11"
```

---

### Constraints

제네릭에 **조건을 주는 방법**입니다.

세부적인 타입을 인자로 받고 추상적인 타입으로 다시 리턴하는 함수는 최악입니다.

예를들어보겠습니다.

> `노동자`라는 interface가 있습니다.
> `월급주기 함수`는 이 `노동자` 타입을 인자로 받아 `노동자`를 리턴해줄 것입니다.
> 따라서 `월급주기 함수`의 타입은 `노동자`가 될 것입니다.

여기서 발생할 수 있는 **문제점**은 `노동자`는 다시 세부적으로 `설계사`, `건축가`, `마케터` 등등 다양한 인스턴스가 될 수 있다는 것입니다.
만약 `건축가`를 월급주기 함수에 전달하면 함수는 월급을 준 뒤 `노동자`를 반환합니다.
여기까지는 문제가 무엇인지 알수 없습니다.
하지만 그 다음 동작에서는 누가 월급을 받았는 지 알 수 없기때문에 만약 월급을 받은 사람의 직종에 따라 다른 복지를 주고 싶어도 할 수 없는 것입니다.

이 때 제네릭을 사용할 수 있습니다.
**_주의할 점_** 은 노동자가 아닌 사장이 인자로 전달될 수 있는 것입니다.
사장은 월급을 주는 사람이 아닌 월급을 받는 사람이므로 오류가 발생합니다.
따라서 제네릭에 조건을 줘야합니다.

**이것이 constraint이며** 다음과 같은 코드로 작성할 수 있습니다.

```tsx
function paySalary<T extends Employee>(employee: T): T {
  employee.pay()
  return employee
}
```

---

> 💡 **_tip_** > _함수에 object가 전달될 때는 다음과 같이 활용할 수도 있습니다._

```tsx
function caculate<T, K extends keyof T>(obj: T, key: K) T[K]{
	return obj[key]
}
```

---

<br>

<br>

# 🔎 type alias와 interface의 차이점

예전에는 타입엘리어스보다 인터페이스가 할 수 있는 기능이 많았기 때문에 인터페이스를 주로 사용했다고 합니다. 하지만 이제는 타입엘리어스의 기능이 매우 강화되고 두 개의 역할이 나눠져있다고 볼 수 있기에 적절하게 잘 섞어서 사용하는 것이 좋습니다.

---

### 비슷한점 : 확장

인터페이스는 extends를 이용해서 확장이 가능하며, 타입은 &을 사용해서 확장이 가능합니다.

---

### 차이점 1. 결합

인터페이스만 가능한 기능입니다. A라는 이름의 인터페이스가 두 개가 있다면 A를 확장한 인터페이스는 두 개의 A 인터페이스를 모두 합한 확장판이 됩니다.

반면 타입은 이런방식으로는 오류가 발생합니다.

---

### 차이점 2. computed property

타입안의 조항을 가져와서 타입을 정의할 수 있습니다.

```tsx
type mobile = {
  phoneNumber: number
  company: string
}

type num = mobile["phonNumber"]
//num은 number타입
```

---

### 📣 결론

API를 구현할 때 어떤 규격을 정의시켜놓고 인스턴스가 이 규격을 따라 정의되도록 하기 위해서는 인터페이스를 사용합니다.
반대로 어떤 데이터를 담을 수 있다고 정의할 때는 type alias를 사용한 것이 좋습니다.\_

<br>

<br>

# 🔎 고급 타입

### 맵 타입

타입 규격은 같은데 타입의 성질이 다른 타입엘리어스가 필요할 수 있습니다.

이것을 하나하나 따로 만들어주기보다 맵타입을 만들어서 이용하면 간단하게 성질만 변화시켜 줄 수 있습니다.

```tsx
type robot = {
  face: boolean
  arms: number
  legs: number
}

type robotOptional1 = {
  face?: boolean
  arms?: number
  legs?: number
}

type Optional<T> = {
  [P in keyof T]?: T[P]
}

type robotOptional2 = Optional<robot>
```

기본적인 맵타입은 타입스크립에 내장된 API를 이용할 수 있습니다

```tsx
type robot = {
  face: boolean
  arms: number
  legs: number
}

function getRobot(robot: Readonly<robot>) {}
```

---

### 조건 타입

받은 타입에 따라 내가 정한 타입을 가져야 한다고 지정해주는 방법입니다.

```tsx
type TypeName<T> = T extends string
? 'string'
: T extends number
? 'number'
: T extends boolean
? 'boolean'
```

위에서 T가 string타입이라면 TypeName은 `"string"` 이라는 문자열을 갖는 타입이 됩니다.
