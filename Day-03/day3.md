# 타입 변환과 단축 평가

## 타입 변환
개발자가 의도적으로 값의 타입을 변환하는 것을 **명시적 타입 변환**이라 한다.
```javascript
var x = 10;
var str = x.toString();
console.log(typeof str, str);   // string 10
console.log(typeof x, x);       // number 10
```
개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동변환 되는것을 **암묵적 타입 변환**이라 한다.
```javascript
var x = 10;
var str = x + '';
console.log(typeof str, str);   // string 10
console.log(typeof x, x);       // number 10
```

## 암묵적 타입 변환
### 문자열 타입으로 변환
```javascript
// Number Type
0 + ''  // "0"
NaN + ''    // "NaN"
Infinity + ''   // "Infinity"

// Boolean Type
true + ''   // "true"
false + ''  // "false"

// null Type
null + ''   // "null"

// undefined Type
undefined + ''  // "undefined"

// symbol Type
(Symbol()) + '' // TypeError: Cannot convert a Symbol value to a string

// Object Type
({}) + ''   // "[object Object]"
Math + ''   // "[object Math]"
[] + ''     // ""
[10,20] + ''    // "10, 20"
(function(){}) + ''     // "function(){}"
Array + ''  // "function Array() { [native code] }"
```

### 숫자 타입으로 변환
```javascript
// String Type
+ ''    // 0
+ '0'   // 0
+ '1'   // 1
+ 'string'  // NaN

// Boolean Type
+true   // 1
+false  // 0

// null Type
+null   // 0

// undefined Type
+undefined  // NaN

// Symbol Type
+Symbol()   // TypeError: Cannor convert a Symbol value to a number

// Object Type
+{}     // NaN
+[]     // 0
+[10, 20]   // NaN
+(function(){})     // NaN
```

### 불리언 타입으로 변환
false로 평가되는 값들:
- false
- undefined
- null
- 0, -0
- NaN
- ''(empty string)

## 명시적 타입 변환
### 문자열 타입으로 변환
```javascript
// 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
// 숫자 타입 => 문자열 타입
String(1);        // -> "1"
String(NaN);      // -> "NaN"
String(Infinity); // -> "Infinity"
// 불리언 타입 => 문자열 타입
String(true);     // -> "true"
String(false);    // -> "false"

// 2. Object.prototype.toString 메서드를 사용하는 방법
// 숫자 타입 => 문자열 타입
(1).toString();        // -> "1"
(NaN).toString();      // -> "NaN"
(Infinity).toString(); // -> "Infinity"
// 불리언 타입 => 문자열 타입
(true).toString();     // -> "true"
(false).toString();    // -> "false"

// 3. 문자열 연결 연산자를 이용하는 방법
// 숫자 타입 => 문자열 타입
1 + '';        // -> "1"
NaN + '';      // -> "NaN"
Infinity + ''; // -> "Infinity"
// 불리언 타입 => 문자열 타입
true + '';     // -> "true"
false + '';    // -> "false"
```

### 숫자 타입으로 변환
```javascript
// 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 숫자 타입
Number('0');     // -> 0
Number('-1');    // -> -1
Number('10.53'); // -> 10.53
// 불리언 타입 => 숫자 타입
Number(true);    // -> 1
Number(false);   // -> 0

// 2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
// 문자열 타입 => 숫자 타입
parseInt('0');       // -> 0
parseInt('-1');      // -> -1
parseFloat('10.53'); // -> 10.53

// 3. + 단항 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
+'0';     // -> 0
+'-1';    // -> -1
+'10.53'; // -> 10.53
// 불리언 타입 => 숫자 타입
+true;    // -> 1
+false;   // -> 0

// 4. * 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
'0' * 1;     // -> 0
'-1' * 1;    // -> -1
'10.53' * 1; // -> 10.53
// 불리언 타입 => 숫자 타입
true * 1;    // -> 1
false * 1;   // -> 0
```

### 불리언 타입으로 변환
```javascript
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 불리언 타입
Boolean('x');       // -> true
Boolean('');        // -> false
Boolean('false');   // -> true
// 숫자 타입 => 불리언 타입
Boolean(0);         // -> false
Boolean(1);         // -> true
Boolean(NaN);       // -> false
Boolean(Infinity);  // -> true
// null 타입 => 불리언 타입
Boolean(null);      // -> false
// undefined 타입 => 불리언 타입
Boolean(undefined); // -> false
// 객체 타입 => 불리언 타입
Boolean({});        // -> true
Boolean([]);        // -> true

// 2. ! 부정 논리 연산자를 두번 사용하는 방법
// 문자열 타입 => 불리언 타입
!!'x';       // -> true
!!'';        // -> false
!!'false';   // -> true
// 숫자 타입 => 불리언 타입
!!0;         // -> false
!!1;         // -> true
!!NaN;       // -> false
!!Infinity;  // -> true
// null 타입 => 불리언 타입
!!null;      // -> false
// undefined 타입 => 불리언 타입
!!undefined; // -> false
// 객체 타입 => 불리언 타입
!!{};        // -> true
!![];        // -> true
```

## 단축 평가
### 논리 연산자를 사용한 단축 평가
논리곱(&&) 연산자와 논리합(||) 연산자는 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환한다.
- true || anything => true
- false || anything => anything
- true && anything => anything
- false && anything => false

### 옵셔널 체이닝 연산자
옵셔널 체이닝 연산자 (?.)는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
```javascript
var elem = null;

var value = elem?.value;
console.log(value);     // undefined

var str = '';

var length = str?.length;
console.log(length);    // 0
```

### null 병합 연산자
null 병합 연산자 (??)는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.
```javascript
var foo = null ?? 'default string';
console.log(foo);   // "default string"

var foo = '' ?? 'default string';
console.log(foo);   // ""
```

# 객체 리터럴

## 객체란
자바스크립트는 객체 기반의 프로그래밍 언어이며, 자바스크립트를 구성하는 거의 "모든 것"이 객체이다.
원시 타입은 단 하나의 값만 나타내지만 객체 타입은 다양한 타입의 값을 하나의 단위로 구성한 복합적인 자료구조이다.
객체는 프로퍼티와 메서드로 구성된 집합체다.
- **프로퍼티**: 객체의 상태를 나타내는 값 (data)
- **메서드**: 프로퍼티를 참조하고 조작할 수 있는 동작 (behavior)

객체 생성 방법:
- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스

## 객체 리터럴에 의한 객체 생성
```javascript
var person = {
    name: 'Lee',
    sayHello: function () {
        console.log(`Hello! My name is ${this.name}.`);
    }
};

console.log(typeof person);     // object
console.log(person);            // {name: "Lee", sayHello: f}

var empty = {};     // empty object
console.log(typeof empty);  // object
```

## 프로퍼티
객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.
- **프로퍼티 키**: 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
  - 식별자 네이밍 규칙을 준수하면 따옴표를 생략할 수 있지만, 그렇지 않으면 반드시 따옴표를 사용해야 한다.
- **프로퍼티 값**: 자바스크립트에서 사용할 수 있는 모든 값
```javascript
var person = {
    firstName: 'Sung',
    'last-name': 'Huh'
};

console.log(person);    // {firstName: "Sung", last-name: "Huh"}
```

## 메서드
프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드라 부른다
```javascript
var circle = {
    radius: 5,      // property
    getDiameter: function() {   // method
        return 2 * this.radius;
    }
};

console.log(circle.getDiameter());  // 10
```

## 프로퍼티 접근
프로퍼티에 접근하는 방법:
1. 마침표 표기법
2. 대괄호 표기법

```javascript
var person = {
    name: 'Lee'
};

console.log(person.name);   // Lee
console.log(person['name']);    // Lee
console.log(person[name]);  // ReferenceError, 대괄호 표기법을 사용하는 경우 키는 따옴표로 감싼 문자열이야 함.
console.log(person.age);    // undefined, 존재 하지 않는 프로퍼티에 접근하면 undefined를 반환.
```

## 프로퍼티 값 갱신
이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.
```javascript
var person = {
    name: 'Lee'
};

person.name = 'Kim';

console.log(person);    // {name: "Kim"}
```

## 프로퍼티 동적 생성
존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당된다.
```javascript
var person = {
    name: 'Lee'
};

person.age = 20;

console.log(person);    // {name: "Lee", age: 20}
``` 

## 프로퍼티 삭제
```javascript
var person = {
    name: 'Lee'
};
person.age = 20;

delete person.age;

console.log(person);    // {name: "Lee"}
```

## 프로퍼티 축약 표현
프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일때 프로퍼티 키를 생략할 수 있다.
```javascript
let x = 1, y = 2;
const obj = { x, y };

console.log(obj);      // {x: 1, y: 2}
```

## 계산된 프로퍼티 이름
문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다.
```javascript
// ES5
var prefix = 'prop';
var i = 0;

var obj = {};

obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj);   // {prop-1: 1, prop-2: 2, prop-3: 3}
// ES6
const prefix = 'prop';
let i = 0;

const obj = {
    [`${prefix}-${++i}`]: i,
    [`${prefix}-${++i}`]: i,
    [`${prefix}-${++i}`]: i,
};

console.log(obj);   // {prop-1: 1, prop-2: 2, prop-3: 3}
```

## 메서드 축약 표현
```javascript
// ES5
var obj = {
    name: 'Lee',
    sayHi: function() {
        console.log('Hi! ' + this.name);
    }
};

obj.sayHi();    // Hi! Lee

// ES6
const obj = {
    name: 'Lee',
    // 메서드 축약 표현
    sayHi() {
        console.log('Hi! ' + this.name);
    }
};

obj.sayHi();    // Hi! Lee
```