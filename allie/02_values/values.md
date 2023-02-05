# 02_OBJECT VALUES

## 2.1 배열

- 자바스크립트의 배열은 어떤 타입의 값이라도 담을 수 있음
- 배열 크기를 미리 정하지 않고도 선언할 수 있음
- 자유롭게 값 추가 가능
- length 프로퍼티 = 마지막 인덱스 + 1 (요소의 개수가 아님)
- 배열 자체도 하나의 객체 -> 문자열 키/프로퍼티 추가 가능 -> but, length가 증가하지는 않음 + `not recommended`
- `주의`: 문자열 키/프로퍼티가 10진수로 타입이 바뀌면 숫자 키를 사용한 것 같은 결과가 초래 ex) test["10"] = 20 -> test[10] = 20 같은 결과

```
  // 구멍 난 배열
  var exp = []
  typeof exp    // "object"

  exp["length"]   // 0, exp.length랑 같음

  exp[0] = 0
  exp[2] = 2
  exp   // [0, empty, 2], exp.length === 3, exp[1] 값이 없음
  exp[1]    // undefined

  exp[1] = undefined
  exp   // [0, undefined, 2], exp.length === 3, exp[1] 값이 undefined

```

```
  // 배열에 문자열 키/프로퍼티 추가
  var exp2 = []

  exp2[0] = 0
  exp2["newKey"] = "test"
  exp2    // [0, newKey: "test"]
  exp2.length   // 1

  // 10진수로 바뀌는 문자열이면 인덱스와 같은 결과
  var exp3 = []

  exp3["5"] = 5
  exp3    // [empty x 5, 5]
  exp3.length     // 6

  // empty element -> forEach, map과 같이 배열을 순회하는 함수를 사용하면 생략
  let arr = [];
  arr[0] = 0;
  arr[1] = 1;
  arr[10] = 10;
  // 0번째 인덱스의 값: 0, 1번째 인덱스의 값: 1, 10번째 인덱스의 값: 10
  arr.forEach((item, index) => console.log(index + '번째 인덱스의 값: ' + item));
```

### 2.1.1 유사 배열 객체

- length 프로퍼티와 인덱싱 된 요소를 가진 객체
- 배열 같지만 배열이 아님 -> `배열 메소드를 사용할 수 없음`
- 대표적인 예 : arguments 객체 (함수 선언문에 넣은 인자 목록)
- `Array.from() 메소드`
  - 유사 배열 객체나 반복 가능한 객체를 얕게 복사해 새로운 Array 객체 return -> 배열 메소드 사용 가능
  - https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from

```
  let test = {
    name: 'test',
    length: 1,
  }

  // 유사 배열 객체에서 배열 메소드 사용하기 방법1 : apply()
  test.push('hello')    // TypeError : test.push is not a function
  Array.prototype.push.apply(test, ['hello'])   // apply() 함수를 사용 -> 유사 배열 객체에서 배열 메소드 사용 가능
  test    // {1: 'hello', name: 'test', length: 2}, 프로토타입 = object

  // 유사 배열 객체에서 배열 메소드 사용하기 방법2 : Array.from()
  function addElements() {
    console.log(arguments);   // Arguments [4, 5, 6, callee, Symbol]
    var array = Array.from(arguments, x => x + x)
    console.log(array)    // [8, 10, 12], length: 3
  }

  addElements(4, 5, 6);
```

---

## 2.2 문자열

- 문자열 = 문자의 배열 ? X
- 문자열 = `불변값` , 배열 = `가변값`

1. 문자열 메소드는 원본을 바로 변경하지 않고 항상 새로운 문자열을 생성한 후 반환 vs 배열은 원본을 바로 수정

```
  var a = "foo"
  a.toUpperCase()   // return "FOO"
  a   // "foo"
  var c = a.toUpperCase()
  c   // "FOO"

  var b = ["foo"]
  b.push("!")   // return 2
  b   // ["foo", "!"]
```

2. 문자열은 불변값이라 배열의 메소드를 쓸 수 없음 -> `문자열 -> 배열 -> 문자열` 작업으로 문자열을 변경할 수는 있음 , `not recommended`

```
  var a = "test"
  // 문자열을 배열로 분리 -> 거꾸로 뒤집음 -> 배열을 합쳐 문자열로 되돌리기
  var b = a.split("").reverse().join("")
  b   // "tset"
```

---

## 2.3 숫자

- 자바스크립트에서 숫자 타입 : number 하나
- 메모리에 IEEE-754 형식을 이용해서 저장
- 콘솔에 0.1을 입력 -> 자바스크립트가 '64 비트 IEEE 754 형식'에 따라 2진법으로 바꿈 -> 그 결과를 다시 10진법으로 바꿔서 화면에 표시

```
  0.1 + 0.2   // 0.30000000000000004 -> '0.1에 가장 가까운 2진법 수를 반올림한 값 + 0.2에 가장 가까운 2전법 수를 반올림'한 값을 반올림한 값
  0.3   // 0.29999999999999998 -> 0.3에 가장 가까운 2진법 수를 반올림한 값
```

### 2.3.1 숫자 구문

1. `Number.toFixed()` : 지정한 소수점 이하 자릿수까지의 숫자를 문자열로 return

```
  var a = 45.33

  a.toFixed(0)    // "45", 반올림
  a.toFixed(1)    // "45.3"
  a.toFixed(2)    // "45.33"
  a.toFixed(3)    // "45.330"
```

2. `지수 표기법` : 'e' (exponent)로 표시 가능

```
  1.123e+5    // 112300, e+는 10의 n 제곱
  1.123e-5    // 0.00001123, e-는 10의 -n 제곱
```

3. `무한대` : 자바스크립트에서 표현할 수 있는 범위(-(2⁵³-1) ~ 2⁵³ -1 )를 넘어가면 Infinity

### 2.3.2 작은 소수 값

- 이진 부동 소수점 숫자의 문제 : 이진 부동 소수점으로 나타낸 수들은 원래의 숫자에 '가까운' 값이지 '일치'하는 값이 아니다

```
  0.1 + 0.2 === 0.3   // false

  numbersCloseEnoughToEqual(0.1 + 0.2, 0.3)   // true
```

### 2.3.3 안전한 정수 범위

- 어쨌든 '안전하게' 표현할 수 있는 (=표현한 값과 실제 값이 정확하게 일치한다고 장담할 수 있는) 정수 값이 -(2⁵³-1) ~ 2⁵³ -1 (9천조)
- 즉, `매우 여유롭다` -> 웬만하면 신경 쓸 필요없음

### 2.3.4 정수인지 확인

- `Number.isInteger()` : 정수면 true, 아니면 false

---

## 2.4 특수 값

### 2.4.1 값 아닌 값

- undefined & null

### 2.4.2 Undefined & Null

- 공통점

1. 각각 타입 값이 유일하다(undefined의 값은 undefined가 유일, null의 값은 null이 유일)

- undefined : 변수를 선언하고 값을 할당하지 않은 상태, 자료형이 없음, 원시 타입

1. typeof undefined === 'undefined'
2. 자바스크립트 엔진이 변수를 초기화할 때 사용하는 값
3. 개발자가 의도적으로 변수에 undefined를 할당하는 것은 not recommended -> 변수에 값이 없다는 것을 명시하고 싶다면 `null 할당`

- null : 변수를 선언하고 빈 값을 할당한 상태, object 자료형, 원시 타입

1. typeof null === 'object'
2. `변수에 값이 없다`는 것을 의도적으로 명시하고자 할 때 사용
3. 변수에 null을 할당한다 = 이전 참조값을 더 이상 참조하지 않겠다, 이전에 참조하던 메모리 공간은 가비지 콜렉션

```
  null === undefined    // false
  null == undefined     // true
  null === null         // true
  null == null          // true
```

- cf) strict equality(===) vs loose equality(==)
  - strict equality
  1. 비교 전에 `형변환되지 않음`
  2. 값과 타입이 같으면 true (NaN 제외, NaN은 다른 모든 값과 같지 않음)
  - loose equality
  1. 두 값을 `공통(common)형으로 변환 후에` 값 비교

### 2.4.3 특수 숫자

- NaN : Not a Number, invalid & failed number

```
  var a = 2 / "foo"
  a   // NaN
  typeof a === "number"   // true, 숫자가 아닌건 아님, 에러난 숫자
```

1. NaN은 `다른 어떤 값과도 동등하지 않음`, 심지어 자기 자신과도 같지 않음
2. `Number.isNaN(a)` : NaN 여부 확인

```
  var a = 2 / "foo"

  a === NaN   // false
  a == NaN    // false
  NaN !== NaN   // true
```

- Infinity
- 0 : +0, -0 존재

### 2.4.4 특이한 동등 비교

- Object.is(a, b) : 두 값이 절대적으로 동등한지 확인 -> 주로 기본 연산자(===, ==)를 쓰고 특이한 동등 비교에만 사용
