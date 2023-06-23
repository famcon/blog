---
title: "Javascript 기본 문법"
category: "TIL"
date: "2022-07-01"
desc: "Javascript 기초 문법들에 대하여 정리"
thumbnail: "../images/javascript-basic.png"
alt: "alt"
---

![](https://images.velog.io/images/c-on/post/cbaa321a-7450-4ebc-ac49-4d66e34b53a9/image.png)

# 🔎 타입

### 블록스코프

`{}` 안에 코드를 작성하면 블록 밖의 global scope에서는 접근할 수 없다.

```jsx
{
  let name = "james"
}
console.log(name) //undefined
```

---

### var를 쓰지 않는 이유

1. 블록 스코프 무시
   - 함수의 블록 외의 나머지 블록은 무시한다.
   - 때문에 선언한적 없는 변수에 값이 할당되어있는 이상한 현상이 발생할 수 있음
2. var hoisting
   - 선언하지 않은 변수가 사용이 가능한 현상이 발생한다.

```jsx
console.log(name) // undefined 출력
var name = "james"
console.log(name) // james 출력
```

값을 할당하기 전 `var name`이 호이스팅돼서 첫줄에서 오류가 발생하지 않는다.

---

### constance를 쓰는 이유

1. 보안성
   - 해커들이 변수의 값을 변경하는 것을 방지
2. 쓰레드 안정성
   - 쓰레드는 변수를 공유하기 때문에 혼란이 발생할 수 있음
3. 프로그래머의 실수
   - 절대 바뀌지 않아야 할 변수의 값을 실수로 바꾸는 일을 방지

---

### 타입종류

- number
- string
- boolean
  - null : 텅 비었음을 할당 `let nothing = null`
  - undefined : 선언은 되었지만 값이 아직 할당되지 않음 `let x // undefined`
- Symbol
  - 변수가 같은 값을 가지고 있더라도 구분되는 특성을 주고싶을 때 사용
  ```jsx
  const a = "hi"
  const b = "hi"
  console.log(a === b) //  true
  const c = Symbol("hi")
  const d = Symbol("hi")
  console.log(c === d) // false
  ```
  - `for`을 사용해서 symbol끼리는 같은 문자열일때 같다고 판단하지만 symbol아닌 변수와는 다르다고 판단하도록 구현할 수도 있다.
  ```jsx
  const c = Symbol.for("hi")
  const d = Symbol.for("hi")
  console.log(c === d) // true
  ```
  - 문자로 변환할 땐 symbol타입 변수뒤에 `.description` 을 붙인다.

---

### 다이나믹 type

자바스크립트에서 `‘7’`과 `5` 를 더하면 5는 문자열로 변환된 후 7과 더해져서 `75`인 문자열이 반환된다.

> `+`는 숫자는 문자열로 변환해서 문자열연산을 수행한다.
> `*`, `/`, `-`는 문자열을 숫자로 변환해서 숫자 연산을 수행한다.

<br>

<br>

# 🔎 연산자

### 논리연산자

`or`연산자는 `true`가 나오면 그곳에서 멈춘다.
반대로 `and`연산자는 `false`가 나오는 곳에서 멈춘다

따라서 연산이 많고 **heavy**한 동작일 수록 뒤쪽에, **심플**한 동작일 수록 앞쪽에 배치하는 것이 좋다.

---

### Equality

`==`는 타입을 검사하지 않는 비교
`===`는 타입까지 비교해주는 연산자

---

### 삼항연산자

`[조건] ? [참일때 반환 값] : [거짓일때 반환 값]` 으로 구성된다.

```js
const beverage = age >= 21 ? "Beer" : "Juice"
```

간단한 비교에서만 사용하기를 권장하며, 여러개의 if..else if문 이라면 `switch..case` 를 사용하는 것이 더 좋다.

---

### switch...case

```js
switch(x) {
  case 'value1':  // if (x === 'value1')
    ...
    [break]

  case 'value2':  // if (x === 'value2')
    ...
    [break]

  default:
    ...
    [break]
}
```

<br>

<br>

# 🔎 배열

### for..of

```jsx
const p = [1, 3, 5, 7, 9]
for (const p of arr) {
  console.log(p)
} // 1\n 3\n 5\n 7\n 9\n
```

---

### forEach

배열 객체의 내장 메서드이다.
forEach는 promise를 기다려주지 않는점을 주의하자.
async await를 선언해도 기다려주지 않는다. 따라서 동기적인 코드는 for문으로 작성해야 한다.

forEach의 콜백함수는 3가지의 인자를 받는다. `(currentValue, index, array)`

- **currentValue**는 돌아가는 상황에서 현재 요소
- **index**는 현재 요소의 인덱스 번호
- **array**는 forEach를 돌리고 있는 배열 자체 를 가진다.

`forEach`에는 `continue`를 사용할 수 없다. `continue`를 사용하려면 for문을 활용해야 한다.

---

### map

배열 안 모든 요소에 **주어진 함수를 호출한 결과**를 모아 새로운 배열을 반환시킨다.

```tsx
const arr = [{ data: 1 }, { data: 2 }, { data: 3 }]
const newarr = arr.map(p => p.data)
// newarr = [1,2,3]
```

---

### filter

**주어진 테스트를 통과하는 요소만 모아**서 새로운 배열을 반환한다.

```tsx
const arr = [4, 2, 36, 8, 3, 1, 9]
const newarr = arr.filter((p, index) => {
  index >= 3 && p > 5
})
// newarr = [8,9]
```

---

### indexOf

입력한 원소와 일치하는 첫번째 인덱스를 반환한다. 없을 경우 -1을 반환한다.

```jsx
const arr = [1, 2, 3, 4, 5]
console.log(arr.indexOf(2)) // 1
console.log(arr.indexOf(9)) // -1
```

만약 원소와 일치하는 마지막 인덱스를 반환하고 싶으면 `lastIndexOf()` 를 사용한다.

---

### splice | slice

- splice : 시작하는 인덱스부터 몇개를 남길 지 인자로 전달
- slice : 시작하는 인덱스부터 (종료인덱스-1)을 슬라이싱해준다. 뒤에서 부터 자를 때 음수를 넣어서 활용이 가능하다.

```jsx
let arr = [3, 6, 9, 8, 7, 5]
console.log(arr.splice(2, 3)) // 9,8,7
arr = [3, 6, 9, 8, 7, 5]
console.log(arr.slice(2, 3)) // 9
arr = [3, 6, 9, 8, 7, 5]
console.log(arr.slice(-6, 3)) //3,6,8
```

---

### combine

`concat` 을 사용하면 결합한 배열을 반환한다.

```jsx
const arr1 = [1, 2, 3]
const arr2 = [4, 5, 6]
const arr3 = arr1.concat(arr2)
```

---

### join

배열 원소들을 콤마와 함께 string으로 결합해서 반환한다. 인자로는 구분자를 콤마대신 지정해줄 수 있다.

```jsx
const arr = ["hello", "world"]
console.log(arr.join()) //hello,world
console.log(arr.join("?")) //hello?world
```

---

### Spread Operator

```jsx
let array = [3, 4, 5]
array.push(...[6, 7]) // [3, 4, 5, 6, 7]
```

<br>

<br>

# 🔎 함수

하나의 함수는 한가지의 일만 하도록 만들어야 한다.

네이밍은 명령형태, 동사형태로 지정해야 하며 이름짓기가 힘들다면 함수안에 너무 많은 동작을 시키고 있지 않은지 의심해볼 필요가 있다.

---

### rest parameter

인자를 받는 것에 제한을 없앨 때 사용된다. 이 때 인자는 함수의 코드블록에서 Array타입으로 사용된다.

```jsx
function printAll(...arr){
	arr.forEach((c)=>{
    	console.log(c)
    }
}
printAll('who', 'are', 'you', '?') // who\n are\n you\n ?\n
```

---

### hoisting이 발생하는 방식과 발생하지 않는 방식

함수를 생성하는 방법은

- **function expression**(함수표현) : 변수에 생성하는 함수를 할당하는 방법이다.
- **function declaration**(함수선언) : 함수 객체에 이름을 지정하여 생성하는 방법이다.

두가지가 있다.

호이스팅은 **함수선언**만 일어난다.
**함수표현**은 함수 생성과 동싱에 한 번 함수가 호출된다.

```jsx
//함수표현
console.log(sum()) // 오류
const sum = function () {
  return 1000
}
```

```jsx
// 함수선언
console.log(sum()) //1000
function sum() {
  return 1000
}
```

<br>

<br>

# 🔎 Object

object는 변수에 할당할 때 **실제 값들을 저장한 메모리 주소**를 변수에 할당한다.

object에 데이터를 추가할 때 `Object.?? = ??` 이 가능하지만 이렇게 동적으로 코딩하면 유지보수가 어렵기 때문에 하지 않는 것이 좋다.

---

### value가져오기

`Object.key` 와 `Object['key']` 를 통해서 값을 가져올 수 있다.

- `Object.key` : 코딩하는 그 순간, 값을 받아오고 싶을 때 사용한다.
- `Object['key']` : **computed properties**라고 부른다. 정확하게 어떤 키가 필요한 지 모르고 런타임에서 결정할 때 사용한다.

---

### for..in

object의 키를 하나하나 뽑아서 반복문을 돌린다.

```jsx
for (key in obj) {
  console.log(key)
}
```

---

### Object.entries()

enumerable속성 [key,value] 쌍을 반환한다.

```jsx
const object1 = {
  a: "somestring",
  b: 42,
}

for (const [key, value] of Object.entries(object1)) {
  console.log(`${key}: ${value}`)
}
```

---

### 복사

```tsx
let obj1 = { a: 0, b: { c: 0 } }
let obj2 = Object.assign({}, obj1)
```

assign은 두 개 이상의 인자를 받는다.
첫번째는 target이며 두번째 이후는 source이다.
sourece가 여러 개일 때 같은 key를 가지면 계속해서 overwriting이 되어서 마지막 key의 value가 적용된다.

얕은 복사에서 obj2의 a속성을 변경해도 obj1에는 영향을 주지 않는다.

하지만 `obj2.b.c` 를 변경하게 되면 `obj1.b.c` 도 함께 변경된다.
객체 안의 객체는 참조값이 복사되었기 때문이다.

```tsx
obj1 = { a: 0, b: { c: 0 } }
let obj3 = JSON.parse(JSON.stringify(obj1))
```

이 트릭 방식으로 깊은 복사를 구현할 수 있다.

<br>

<br>

# 🔎 프로미스

### State

기능이 수행 중인지, 수행 후 성공인지 실패인지를 나타낸다.

pending → fulfilled || rejected

---

### 생산자

새 포르미스가 만들어질 때 전달된 executor함수가 자동으로 실행된다.

```jsx
const promise = new Promise((resolve, reject) => {
  console.log("executor 자동 실행")
  setTimeOut(() => {
    resolve("수행")
  }, 1000)
})
```

---

### 소비자

- then : 프로미스가 수행이 끝나고 난 뒤 마지막으로 반환하는 값을 받아온다.

  ```jsx
  promise.then(value => {
    console.log(value)
  })
  ```

- catch : 에러가 발생했을 때 캐치한다.

  ```jsx
  const promise = new Promise((resolve, reject) => {
    console.log("executor 자동 실행")
    setTimeout(() => {
      reject("기능 수행 끝")
    }, 1000)
  })

  promise
    .then(value => {
      console.log(value)
    })
    .catch(value => {
      console.log(`error 발생 ${value}`)
    })
  ```

chaining이 가능한 이유는 then이 반환한 데이터 역시 promise이기 때문이다.

---

### finally

성공과 실패에 상관없이 무조건 수행해야 할 기능을 chaining해줄 수 있다.

---

### 병렬처리

```jsx
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms))
}

async function getApple() {
  await delay(1000)
  return "apple"
}

async function getBanana() {
  await delay(1000)
  return "banana"
}

async function getFruit() {
  const a = await getApple()
  const b = await getBanana()
  return `I got a ${a}, ${b}`
}
getFruit().then(console.log) // 2초 걸림
```

위 예제에서 2개의 프로미스에 await를 걸어 순차적으로 진행하면 연관되지않은 아이들이 서로를 기다려줘야하는 문제가 발생한다.

<br>

```jsx
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms))
}

async function getApple() {
  await delay(1000)
  return "apple"
}

async function getBanana() {
  await delay(1000)
  return "banana"
}

async function getFruit() {
  const applePromise = getApple()
  const bananaPromise = getBanana()
  const a = await applePromise
  const b = await bananaPromise
  return `I got a ${a}, ${b}`
}
getFruit().then(console.log) // 1초 걸림
```

새 프로미스를 하나 만들고 그 안에 비동기함수를 await를 걸어줌으로써 효율을 극대화 할 수 있다.

프로미스는 생성과 동시에 그 안의 executor를 실행하기 때문에 두 비동기가 각각 동작하지만 밖의 프로미스는 둘 다 await가 걸리게 된다. 이것을 **병렬처리**라고 한다.

<br>

```jsx
function getAllfruits() {
  return Promise.all([getApple(), getBanana()]).then(fruits => {
    console.log(fruits)
    fruits.join("+")
  })
}

getAllfruits().then(console.log)
```

프로미스 API를 사용하면 더 깔끔하게도 가능하다.
