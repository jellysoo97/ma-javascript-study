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

- String() : 문자열을 감싸는 wrapper 객체를 생성

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
