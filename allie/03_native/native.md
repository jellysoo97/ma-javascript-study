# 03_NATIVE

- 특정 환경(브라우저 등의 클라이언트 프로그램)에 종속되지 않은 ECMAScript 내장 객체, 내장 함수
- Window는 네이티브가 아님

1. String()
2. Number()
3. Boolean()
4. Array()
5. Object()
6. Function()
7. RegExp()
8. Date()
9. Error()
10. Symbol()
11. 등등..

- `String()` : 문자열을 감싸는 wrapper 객체를 생성

```
  var test = new String("test")   // 원시값 "test"를 감싼 객체 래퍼

  test    // String {'test'}, {0:"t", 1:"e", 2:"s", 3:"t"}
  typeof test   // "object"
```

## 3.1 내부 [[Class]]

- typeof가 'object'인 값들은 [[Class]]라는 내부 프로퍼티가 있음
- 직접 접근할 수는 없고 Object.prototype.toString.call(a)의 방식으로 호출
- null, undefined도 있음

```
  Object.prototype.toString.call(null)    // "[object Null]"
  Object.prototype.toString.call(undefined)   // "[object Undefined]"
```

- 단순 원시값(문자열, 숫자, 불리언 등)은 해당 객체 래퍼로 자동 박싱됨

```
  Object.prototype.toString.call(42)    // "[object Number]"
```

---

## 3.2 래퍼 박싱하기

- 원시값에는 프로퍼티나 메소드가 없으므로 .length, .toString()으로 접근하려면 객체 래퍼로 감싸줘야 함 -> 자바스크립트가 알아서 박싱해줌
- 그러니 굳이 new String("abc"), new Number(42)처럼 감싸지 말자

```
  var test = "test"

  test.length   // 4, 알아서 객체 래퍼로 박싱해줘서 가능함
  test.toUpperCase()    // "TEST", 알아서 객체 래퍼로 박싱해줘서 가능함
```

---

## 3.3 언박싱

- `valueOf()` : 객체 래퍼의 원시값 추출

```
  var a = new String("abc")
  var b = new Number(42)
  var c = new Boolean(true)

  a.valueOf()   // "abc"
  b.valueOf()   // 42
  c.valueOf()   // true
```

---

## 3.4 네이티브, 나는 생성자다

> - 생성자 함수: js에서 객체를 생성하기 위해 사용되는 특수한 함수
>   - Date 객체 생성 : new 연산자 + Date() 생성자 함수
> - js는 Array, Boolean, Error, Function, Number, Date와 같은 여러 내장 생성자가 제공

- 확실히 필요해서 쓰는게 아니라면 생성자는 가급적 쓰지 않는 편이 좋음 -> 오류가 많이남

### 3.4.1 Array()

```
  var a = new Array(1, 2, 3)
  a   // [1, 2, 3]

  var b = [1, 2, 3]
  b   // [1, 2, 3]
```

- `인자가 숫자 하나면 배열의 크기를 미리 정하는 기능` -> 해당 숫자가 원소가 되는 배열을 생성하는게 아님
- `이상하다`
  - 빈 슬롯이지만 length는 1이상인 경우 발생
  - 브라우저마다 다름

```
  // 크롬 기준
  var a = new Array(2)

  a.length    // 2
  a   // [empty x 2]

  var test1 = new Array(3)
  var b = [undefined, undefined, undefined]
  var c = []
  c.length = 3

  a   // [empty x 3], length: 3
  b   // [undefined, undefined, undefined], length: 3
  c   // [empty x 3], length: 3
```

### 3.4.2 Object(), Function() and RegExp()

- 이 생성자들 또한 `선택적`
- `new Object()` : 사용할 일이 거의 없음, 리터럴 형태로 한번에 여러 프로퍼티 지정 가능하니까 굳이 싶음
  - var a = {name: 'allie', age: 20}
- `new Function()` : 함수의 인자나 내용을 동적으로 정의해야 할 때만 드물게 사용
- `new RegExp()` : `리터럴 형식으로 정의할 것을 적극 권장`
  - 가독성이 좋음
  - 성능상 이점 -> 자바스크립트 엔진이 실행 전 정규 표현식을 미리 컴파일 후 캐시

### 3.4.3 Date() and Error()

- `리터럴 형식이 없어서 유용`

1. `new Date()`

- 날짜/시각을 인자로 받음
- 인자가 없으면 현재 날짜/시각으로 대체

2. `new Error()`

-
