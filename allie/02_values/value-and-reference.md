### 2.5 값 vs 레퍼런스

1. 값에 의한 전달(passed by value)이 일어나는 5가지 데이터 타입(`=원시타입`) : `boolean, null, undefined, string, number`
2. 참조에 의한 전달(passed by reference)이 일어나는 3가지 데이터 타입(`=객체 objects`) : `array, function, object`

- 원시타입
  - 어떠한 원시타입이 변수에 할당되면 그 변수는 원시타입을 가진 변수
  - 이 변수를 다른 변수에 할당할 때, 새로운 변수에 `값을 복사`
  - 값이 같더라도 각각의 변수들은 아무런 관계도 없음 -> 값을 바꾸더라도 다른 변수에는 영향X

```
  var x = 10;
  var y = 'abc';

  var a = x;
  var b = y;

  console.log(x, y, a, b);    // 10, 'abc', 10, 'abc' -> 다 다른 분리된 값
```

- 객체

  - 객체 값이 할당된 변수들은 `그 값으로 향하는 참조`를 갖게 됨
  - 참조(reference) : 메모리에서 객체의 위치를 가리킴, 변수는 실제로 값을 가지고 있는게 아님
  - `arr = [1, 2, 3]` -> 메모리 어딘가에 배열을 만든 것 -> 변수 arr이 갖는 값은 배열이 위치한 주소
  - 변수가 가지는 값 (=주소)는 정적임, 바뀌지 않음 -> `변수가 가지는 값이 바뀌는게 아니라 메모리 속 배열 값이 바뀌는 것`

  ```
    var arr = []    // variable : arr / value : <#001> / address : #001 / object : []
    arr.push(1)     // variable : arr / value : <#001> / address : #001 / object : [1]
  ```

  - `참조로 할당`
    - 참조 타입의 값이 다른 변수로 복사될 때, 새로운 변수에 `값의 주소가 복사`
    - 객체는 값 대신 참조로 복사

  ```
    var arr = [1]       // variable : arr / value: <#002> / address : #002 / object : [1]
    var arrCopy = arr   // variable : arrCopy / value: <#002>

    arr.push(2)
    console.log(arr, arrCopy)   // [1, 2], [1, 2]
  ```

  - `참조로 재할당`
    - 오래된 참조를 대체
    - `오래된 참조 즉, 더이상 사용할 수 없는 참조는 가비지 콜렉션`
    - cf) 가비지 콜렉션 : 자바스크립트 엔진이 사용되지 않는 객체를 메모리로부터 안전하게 지워버리는 것
    ```
      var obj = {first:'ref'}
      obj = {second:'ref2'}    // {first:'ref}는 가비지 콜렉션, 더이상 접근 불가
    ```
    ![image](https://velog.velcdn.com/post-images%2Fjakeseo_me%2F449e94d0-546e-11e9-a095-d742f66aa765%2Farr5.png)
  - `함수를 통한 파라미터 전달`

    - 원시 값들을 함수로 전달할 때, 함수는 `값을 복사`하여 파라미터로 전달 = 순수함수
    - 주변 스코프의 상태에 영향을 주지 않음
    - 함수 안에서 만들어진 변수들은 함수가 반환되는 즉시 가비지 콜렉션 처리

    ```
      var hundred = 100;
      var two = 2;

      function multiply(x, y) {   // x의 value : 100 / y의 value : 2
        return x * y;
      }

      var twoHundred = multiply(hundred, two);
    ```

    - 객체 값들을 함수로 전달할 때, 함수는 주변 스코프들의 상태를 변화시킬 수 있음 = 순수함수 X
      - 함수가 배열 참조값을 가진 변수를 받음 -> 변수가 가리키는 배열에 push를 수행 -> 주변 스코프에 존재하는 변수 + 참조 + 참조하는 배열 값이 변함
    - side-effect 생길 수 있음 -> 그래서 `객체를 복사하고 원본 대신 복사된 배열로 작업하는게 좋음`

> https://velog.io/@jakeseo_me/2019-04-01-1904-%EC%9E%91%EC%84%B1%EB%90%A8-2bjty7tuuf
