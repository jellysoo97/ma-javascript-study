# 01_TYPE

## 1.1 타입

- 어떤 값을 다른 값과 분별할 수 있는 고유한 내부 특성의 집합

## 1.2 내장타입

- 자바스크립트의 7가지 내장타입(원시타입)

1. null
2. undefined
3. boolean
4. string
5. number
6. object
7. symbol > ES6 NEW!

- typeof 연산자로 확인 -> 문자열 return

```
  typeof undefined === "undefined"    // true

  typeof null === "object"    // true
  var a = null
  !a && typeof a === "undefined"    // true, 이래야 a가 null 값을 가지는지 정확하게 확인가능

  typeof [1,2,3] === "object"   // true, array는 object의 하위타입
  typeof function a () {...} === "function"   // true, function은 object의 하위타입

  function a (b, c) {...}
  a.length    // 2, 인자의 개수 return
```

## 1.3 값은 타입을 가진다

- 값에는 타입이 있지만 변수에는 정해진 타입이 없음
- 이 변수는 어떤 타입? = 이 변수에 들어있는 값의 타입은?

### 1.3.1 undefined (값이 없는) vs undeclared (선언되지 않은)

- undefined: 변수가 선언되었으나 현재 아무 값도 할당되지 않은 상태
- undeclared: 선언조차 되지 않은 상태
- 자바스크립트는 undefined, undeclared를 대충 섞어버림
  - undeclared 에러메세지에 not defined
  - undeclared 변수의 타입이 undefined

```
  var a;
  a;    // undefined
  b;    // ReferenceError("b is not defined")

  var a;
  typeof a    // "undefined" -> undefined
  typeof b    // "undefined" -> undeclared
```

### 1.3.2 선언되지 않은 변수

- 전역 변수 체크하기

1. typeof 안전가드 : typeof 선언되지 않은 변수 -> 브라우저가 오류 처리하지 않음 -> 이걸 이용해서 선언되지 않은 변수인지 체크할 때 유용

```
  // error, DEBUG는 선언되지 않음
  if (DEBUG) {
    console.log("start debug mode")
  }

  // good
  if (typeof DEBUG !== "undefined") {
    console.log("start debug mode")
  }

  // 변수 체크, abc 변수가 있으면 그대로 사용하고 없으면 새로 정의
  var tester = (typeof abc !== "undefined") ? abc : "newValue"
```

2. window 객체를 통한 참조 : 전역 변수는 전역 객체(브라우저의 경우 window)의 프로퍼티이기 때문

- but, window 객체로만 호출하지 않는 다중 자바스크립트 환경일수도 있으니 (브라우저 외의 환경에서 실행될수도 있으니) 가급적 삼가는 것이 좋음

```
  // 프로퍼티가 존재하지 않아도 ReferenceError 나지 않음
  if (window.DEBUG) {
    ...
  }
```
