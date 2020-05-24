# 자바스크립트 관련 질문

## 목차
- [Q. 기본 문법](#q-기본-문법)
- [Q. ES6(ECMAScript 6)](#q-es6ecmascript-6)


## Q. 기본 문법
### 기본 타입
자바스크립트에서 기본 타입은 총 5가지 입니다.
- **숫자**: 자바스크립트에서는 정수, 소수 등 모든 숫자를 64비트 부동 소수점 형태로 저장한다.(나눗셈 주의)
- **문자열**: 문자 및 문자열 선언은 "", '' 모두 가능하다.
- **불린값**
- **null**: null의 타입은 object이고 `null`을 직접 할당하므로써 명시적으로 "비어있음"을 나타낸다.
- **undefined**: undefined은 그 자체로 타입이며, 값이다. 변수에 아무런 값을 할당 안했을 때 `undefined` 값이 할당된다.

### `==` 연산자 VS `===` 연산자
`==` 연산자는 동등 연산자이고, `===` 연산자는 일치 연산자를 말합니다.

```js
console.log(1 == '1');    // true
console.log(1 === '1');   // false
```

`==` 연산자는 비교하려는 피연산자의 타입이 다를 경우 타입 변환을 거친 다음 비교합니다. 반면에 `===` 연산자는 피연산자의 타입이 다르더라도 타입 변환을 하지 않고 비교합니다. 대부분의 자바스크립트 컨벤션은 `==` 연산자로 인해 잘못된 결과를 가져올 수 있으므로, `===` 연산자를 사용하도록 권고합니다.

### `!!` 연산자
자바스크립트는 `!` 연산자와 `!!` 연산자 두 가지가 있습니다. `!` 연산자는 다른 언와 같고, `!!` 연산자는 피연산자를 불린값으로 변환합니다.

```js
console.log(!!0);           // false
console.log(!!1);           // true
console.log(!!'string');    // true
console.log(!!'');          // false
console.log(!!null);        // false
console.log(!!undefined);   // false
console.log(!!{});          // true
```


## Q. ES6(ECMAScript 6)
ECMAScript는 자바스크립트 언어의 표준을 말합니다. ECMAScript 6는 2015년 6월에 업데이트되었으며, 6번 째 에디션입니다. 이전 버전과의 변경된 점은 아래와 같습니다.

### class 키워드
자바스크립트도 C++, Java와 같이 `class` 키워드로 객체를 선언할 수 있게 되었습니다.

```js
class Person {
    constructor (id, name) {
        this.id = id
        this.name = name
    }
    toString() {
        return `(${this.id}, ${this.name})`
    }
}

class Student extends Person {
    constructor (id, name, age) {
        super(id, name)
        this.age = age
    }
    toString() {
        return super.toString() + ' and ' + this.age
    }
}
```

### let & const
ES6 이전에는 변수 선언에 `var` 키워드 뿐이었습니다. 이 변수의 스코프는 함수 전체라는 문제점도 있었습니다.

ES6부터 `let`, `const`가 새로 생겼습니다. 둘 다 스코프는 블록 스코프를 가지고 있습니다. 블록 스코프란 `{}` 블록 내부에서 변수를 선언하면 그 변수는 해당 블록에서만 사용할 수 있습니다. 블록 스코프의 장점은 해당 변수의 영향을 최소화하면서 코드의 이해를 높이고 디버깅을 좀 더 쉽게 할 수 있다고 생각합니다.

`let`으로 선언 시 값을 변경할 수 있는 변수이고, `const`로 선언 시 초기화 후 값을 변경 및 재선언할 수 없는 상수가 됩니다.

```js
const schoolName = "ABC"

schoolName = "CBA"  // Error
```

```js
function test() {
  let x = "a"
  if (true) {
    let x = "b"
    console.log(x);  // b
  }
  console.log(x);  // a
}
```

### Arrow Functions
Arrow Function을 통해 함수의 선언이 좀 더 간결해졌습니다. 기존의 `function() {}`와의 차이점은 `this` 바인딩 방식입니다.

Arrow Function은 자신만의 `this`를 생성하지 않습니다. 따라서 기존의 방식과의 차이점은 다음과 같습니다.
- 기존 방식

```js
function NumberEx() {
  var that = this
  that.num = 0;
  setInterval(function add() {
    // setInterval 안에서의 this 는 NumberEx의 this가 아니므로 다른 변수에 this 를 지정하여 씁니다.
    that.num++;
    console.log(that.num);
  }, 1000);
}
```

- Arrow Function

```js
function NumberEx() {
  this.num = 0

  setInterval(() => {
    this.num++ // this is from NumberEx
  }, 1000)
}
```

Arrow Function은 내부에서 `this`를 생성하지 않기 때문에 바로 인접한 함수의 스코프에서 `this`를 사용하게 됩니다.

### Modules
Export, Import 를 이용해 function이나 variables 들을 다른 곳에서 사용할 수 있습니다.

```js
//  utility.js
export const squares = (arr) => { return arr.map(x => x * x) }

// math.js
import { squares } from "utility"
console.log(squares([1,2,3])) // [1,4,9]
```

### Promises
자바스크립트는 Event Driven 방식으로 비동기 프로그래밍을 많이 사용하고 있습니다. 따라서 콜백 함수가 매우 많아졌고, 콜백 함수 내부에서 중첩적으로 콜백 함수를 호출하는 Callback Hell이라는 현상이 발생했습니다. 이를 해결하기 위해 가독성을 높이고 중첩된 콜백을 줄이기 위해 Promise가 나왔습니다.

Promise는 세 가지 상태를 가집니다.
- 대기중(pending)
- 이행됨(fulfilled)
- 거부됨(rejected)

Promise를 생성하는 방법은 다음과 같습니다.

```js
var promiseTest = (num) => {
    return new Promise((resolve, reject) => {
        if (num > 3) {
            resolve(num)
        } else {
            reject("err")
        }
    })
}

promiseTest(5)
    .then(val => console.log(val)) // 5
    .catch(err => console.log(err))
```

`new Promise()`는 resolve와 reject 매개변수를 갖습니다. resolve가 호출되면 `then()`이 실행되고, reject가 호출되면 `catch()`가 실행됩니다.

### Template String
ES6부터 백틱(`)를 사용하여 문자열을 만들 수 있습니다. 백틱을 사용하면 문자열 내부에 변수를 쉽게 추가할 수 있습니다.

```js
// num1, num2, result 변수가 선언되어 있다고 가정
// 기존 방식
var string_before = num1 + ' + ' + num2 + ' = ' + result;

// 백틱 사용
const string_after = `${num1} + ${num2} = ${result}`;
```

### Reference
- <https://woowabros.github.io/experience/2017/12/01/es6-experience.html>