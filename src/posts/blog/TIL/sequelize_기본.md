---
title: "Sequelize 기본"
category: "TIL"
date: "2022-03-01"
desc: "sequelize"
thumbnail: "../images/sequelize.png"
alt: "alt"
---

## 소개

시퀄라이즈는 프로미스를 기반으로 한 nodeJS의 ORM 툴이다.

## 시작

`npm i sequelize`

설치 하고, 사용하려는 데이터 베이스 드라이버도 수동으로 설치한다.

난 mysql 사용하니까 `npm i mysql2`

## 연결

데이터 베이스에 연결하기 위해선 먼저 sequelize 인스턴스를 생성해야 한다.

인스턴스란 어떤 클래스의 하나의 객체를 말한다.

sequelize 인스턴스를 통해서 연결 매개변수를 전달하거나 단일 연결 URI를 전달한다.

이는 new를 사용해서 새로운 객체를 만들고 그것을 인스턴스라고 하는 것.

이때 생성하면서 URI를 넘겨서 인스턴스와 DB를 연결할 수도 있고, 파라미터를 전달하여 인스턴스를 생성할 수도 있다.

하나의 sequelize 인스턴스는 하나의 데이터베이스에 대한 **연결**을 나타낸다는 규칙을 가진다.

## 모델

모델은 sequlize의 핵심이다.

모델은 **테이블**을 추상화한것이다.

추상화이기 때문에 모델이 바뀌었다고 실제 테이블이 바뀌지는 않는다.

하지만 테이블을 모델과 같도록 만들수가 있다.

이러한 방식으로 테이블을 수정한다.

모델을 정의하는 방법은 두가지다

1. **define**

   모델을 생성하는 함수로 인자를 전달해서 테이블 이름, 속성 등을 설정한다.

2. **init**
   sequlize에는 Model이라는 클래스가 있다. 이를 extends해서 인스턴스를 생성한다.

본질적으로 둘은 같은 동작을 수행하므로 문서에서 같은 접근법이라고 한다.

동기화는 model.sync를 통해 각각의 모델을 동기화시켜서 테이블을 생성할 수 있지만

서버를 시작할 때 한번에 동기화 하는 API를 사용해서 프로미스므로 then을 사용해서 동기화가 끝나면 서버가 시작되도록 설정하는 것이 일반적이다.

## 모델 인스턴스 API

모델 클래스로 어떤 테이블을 추상화한 모델을 만들었다면 그 모델은 다양한 API를 가진다.

build, save, create등이 있고 값을 단순히 증가 또는 감소시키는 API도 있다.

reload를 사용하면 모델을 새로고침 할 수 있다.

## 모델 쿼리

`attributes`를 설정해서 일부 열만 가져올 수 있다.

이때 가져올 것을 정할 수도 있지만 안에 다시 excludes를 설정해서 가져오지 않을 것만 따로 설정할 수도 있다.

`where` 절은 `Op` 가 설정되지 않으면 기본적으로 `[Op.eq]` 를 가정한다.

또한 여러개의 object를 넣으면 자동으로 `[Op.and]`를 가정합니다.

배열을 전달하면 암시적으로 `[Op.in]` 연산자가 사용된다.

`order`과 `group`도 가능하며 `limit`과 `offset`으로 데이터를 제한, 스킵할 수 있다.

**✅ count, max, min, sum**

count는 `Op` 를 통해 사용할 수 있고 나머지는 모델 인스턴스에 내장된 API로 바로 적용할 수 있다.
_ex) User.max('age')_

## Getter, Setter....

음.. [https://jeonghwan-kim.github.io/sequelize-model/](https://jeonghwan-kim.github.io/sequelize-model/) 여기 잘 정리 되어 있긴 한데..

서버에서 처리할 수 있고, 그게 더 빠르게 동작할 듯..

## 검증과 제약

이 부분은 DB보다 서버에서 로직을 짜야 더 빠를 것이기 때문에 따로 정리하지 않을 것.

## Raw Query

원시 퀴리는 SQL문법을 사용한 쿼리를 말한다.

쿼리를 날리면 [results, metadata] 이렇게 두개의 arguments를 받게 된다.

result에는 요청한 데이터가, metadata에는 가져온 row의 길이를 담는다.

쿼리옆에 후속 설정을 할 수 있는데 쿼리 타입을 지정하면 metadata없이 전달받을 수 있다.

또, 모델이 무엇인지를 전달하면 반환된 데이터는 해당 모델의 인스턴스가 된다.

## 관계 association

일대일 관계를 만들기 위해선 `hasOne`, `belongsTo` 를 함께 사용한다.

일대다 관계를 만들기 위해선 `hasMany`, `belongsTo` 를 함께 사용한다.

다대다 관계를 만들기 위해선 `belongsToMany` 를 사용한다.

belongsToMany는 연결테이블을 함께 지정해줘야 한다.

**✅ 일대일 관계**

`hasOne` 설정 시 옵션으로 ON DELETE, ON UPDATE가 있다.

이들은 기본적으로 ON DELETE: SET NULL, ON UPDATE : CASCADE로 설정이 되어있다.

CASCADE는 종속이란 의미로 부모가 변경될 때 종속된 무언가도 함께 변경되도록 한다.

**✅ 일대다 관계**

`hasMany` 라고 설정해주어야 ON DELETE, ON UPDATE 같은 것들이 사용가능하다.

**✅ 다대다 관계**

다대다 관계는 코드로 보는게 더 직관적이고 이해하기 쉬울것같다.

```jsx
const Movie = sequelize.define("Movie", { name: DataTypes.STRING })
const Actor = sequelize.define("Actor", { name: DataTypes.STRING })
Movie.belongsToMany(Actor, { through: "ActorMovies" })
Actor.belongsToMany(Movie, { through: "ActorMovies" })
```

이 코드는 다음 SQL과 같은 ActorMovies라는 테이블을 만들어낸다.

```sql
CREATE TABLE IF NOT EXISTS "ActorMovies" (
  "createdAt" TIMESTAMP WITH TIME ZONE NOT NULL,
  "updatedAt" TIMESTAMP WITH TIME ZONE NOT NULL,
  "MovieId" INTEGER REFERENCES "Movies" ("id") ON DELETE CASCADE ON UPDATE CASCADE,
  "ActorId" INTEGER REFERENCES "Actors" ("id") ON DELETE CASCADE ON UPDATE CASCADE,
  PRIMARY KEY ("MovieId","ActorId")
);
```
