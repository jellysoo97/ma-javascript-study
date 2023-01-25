# 02_OBJECT VALUES

## 2.1 배열

- 자바스크립트의 배열은 어떤 타입의 값이라도 담을 수 있음
- 배열 크기를 미리 정하지 않고도 선언할 수 있음
- 자유롭게 값 추가 가능
- lenght 프로퍼티 = 마지막 인덱스 + 1 (요소의 개수가 아님)
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

## 2.1.1 유사 배열 객체

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

2. 문자열은 불변값이라 배열의 메소드를 쓸 수 없음
