# 객체 리터럴

## 객체란?
자바스크립트는 객체 기반의 프로그래밍 언어이며, 자바스크립트를 구성하는 거의 "모든 것"이 객체다.
객체는 0개 이상의 프로퍼티로 구성되 집합이며, 프로퍼티느 키와 값으로 구성된다.
프로퍼티 값이 함수일 경우, 일반 함수와 구분학 위해 메서드라 부른다.

```javascript
var counter = {
   num: 0,
   increase: function () {
      this.num++;
   }
};
```

## 객체 리터럴에 의한 객체 생성
- 객체 리터럴 -> 가장 일반적인 방법
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

```javascript
var person = {
  name: 'Lee',
  sayHello: function () {
    console.log( 'Hello! My name is ${this.name}.' );
  }
};

console.log( typeof person ); // object
console.log( person ); // {name: "Lee", sayHello: f}

var empty = {}; // empty object
console.log(typeof empty}; // object
```

## 프로퍼티
```javascript
var person = {
  // 프로퍼티 키는 name, 프로퍼티 값은 'Lee'
  name: 'Lee',
  // 프로퍼티 키는 age, 프로퍼티 값은 20
  age: 20
};

var obj = {};
var key = 'hello';

obj[key] = 'world';

console.log(obj); // {hello: "world"}

var foo = {
  name: 'Lee',
  name: 'Kim'
};

console.log(foo); // {name: "Kim"}
```

## 메서드
```javascript
var circle = {
  radius: 5, // <- 프로퍼티
  getDiameter: function() { // <- 메서드
    return 2 * this.radius; // this는 circle을 가리킨다.
  }
};

console.log(circle.getDiameter()); // 10
```

## 프로퍼티 접근

- 마침표 프로퍼티 접근 연산자(.)를 사용하는 **마침표 표기법**
- 대괄호 프로퍼티 접근 연산자([ … ])를 사용하는 **대괄호 표기법**

```jsx
var person = {
	name: "Lee"
};

// 마침표 표기법
console.log(person.name); // Lee

// 대괄호 표기법
console.log(person['name']); // Lee
```

## 프로퍼티 값 갱신

```jsx
var person = {
	name: 'Lee'
};

// person 객체에 name 프로퍼티가 존재하므로 name 프로퍼티의 값이 갱신된다.
person.name = 'Kim';

console.log(person); // {name: "Kim"}
```

## 프로퍼티 동적 생성

```jsx
var person = {
	name: 'Lee'
};

// person 객체에는 age 프로퍼티가 존재하지 않는다.
// 따라서 person 객체에는 age 프로퍼티가 동적으로 생성되고 값이 할당된다.
person.age = 20;

console.log(person); // {name: "Lee", age: 20}
```

## 프로퍼티 삭제

```jsx
var person = {
	name: 'Lee'
};

// 프로퍼티 동적 생성
person.age = 20;

delete person.age;

console.log(person); // {name: "Lee"}
```

## ES6에서 추가된 객체 리터럴의 확장 기능

### 프로퍼티 축약 표현

ES6에서는 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략 할 수 있다.

```jsx
let x = 1, y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

### 계산된 프로퍼티 이름

ES6에서는 객체 리터럴 내부에서도 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성할 수 있다.

```jsx
const prefix = 'prop';
let i = 0;

const obj = {
	[`${prefix}-${++i}`]: i
	[`${prefix}-${++i}`]: i
	[`${prefix}-${++i}`]: i
};

console.log(obj);  // {prop-1: 1, prop=2: 2, prop-3: 3}
```

### 메서드 축약 표현

ES6에서는 메서드를 정의할 때 function 키워드를 생략한 축약 표현을 사용할 수 있다.

```jsx
const obj = {
	name: 'Lee',
	sayHi() {
		console.log('Hi!' + this.name);
	}
};

obj.sayHi(); // Hi! Lee
```

# 원시 값과 객체의 비교

## 원시 값

### 변경 불가능한 값

- 원시 타입의 값, 즉 원시 값은 변경 불가능한 값이다.
- 불변성을 갖는 원시 값을 할당한 변수는 재할당 이외에 변수 값을 변경할 수 있는 방법이 없다.

### 문자열과 불변성

```jsx
var str = 'string';

str[0] = 'S';

console.log(str); // string
```

### 값에 의한 전달

- 변수에 원시 값을 갖는 변수를 할당하면 할당받는 변수에는 할당되는 변수의 원시 값이 복사되어 전달된다.
- “값에 의한 전달”도 사실은 값을 전달하는 것이 아니라 메모리 주소를 전달한다.
- 단, 전달된 메모리 주소를 통해 메모리 공간에 접근하면 값을 참조할 수 있다.

## 객체

### 변경 가능한 값

- 객체(참조) 타입의 값, 즉 객체는 변경 가능한 값이다.

### 참조에 의한 전달

- 객체를 가리키는 변수를 다른 변수에 할당하면 원본의 참조 값이 복사되어 전달된다.

# 함수

## 함수란?

함수는 일련의 과정을 문(statement)으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것이다.

## 함수 리터럴

구성요소:

- 함수 이름
- 매개변수 목록
- 함수 몸체

## 함수 정의

- 함수 선언문

```jsx
function add(x, y){
	return x + y;
}
```

- 함수 표현식

```jsx
var add = function(x, y){
	return x + y;
};
```

- Function 생성자 함수

```jsx
var add = new Function('x', 'y', 'return x+y');
```

- 화살표 함수(ES6)

```jsx
var add = (x, y) => x + y;
```

## 함수 호출

### 매게변수와 인수

```jsx
function add(x, y) {
	console.log(x, y); // 2, 5
	return x + y;
}

add(2, 5);

// add 함수의 매개변수 x, y는 함수 몸체 내부에서만 참조할 수 있다.
console.log(x, y); // ReferenceError: x is not defined

console.log(add(2)); // NaN

console.log(add(2, 5, 10)); // 7
```

### 인수 확인

자바스크립트의 경우 함수를 정의할 때 적절한 인수가 전달되었는지 확인할 필요가 있다.

```jsx
function add(x, y) {
	if (typeof x !== 'number' || typeof y !== 'number' ){
		throw new TypeError('인수는 모두 숫자 값이어야 합니다.');
	}

	return x + y;
}

console.log(add(2)); // TypeError
console.log(add('a', 'b')); // TypeError
```

ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다.

```jsx
function add(a=0, b=0, c=0){
	return a + b + c;
}

console.log(add(1, 2, 3)); // 6
console.log(add(1, 2)); // 3
console.log(add()); // 0
```

### 매개변수의 최대 개수

이상적인 함수는 한 가지 일만 해야 하며 가급적 작게 만들어야 한다.

매개변수는 최대 3개 이상을 넘지 않는 것을 권장한다.

### 반환문

return 키워드와 표현식 (반환값)으로 이뤄진 반환문을 사용해 실행 결과를 함수 외부로 반환(return)할 수 있다.

```jsx
function multiply(x, y){
	return x * y; // 반환문
}

var result = multiply(3, 5);
console.log(result); // 15
```

## 다양한 함수의 형태

### 즉시 실행 함수

함수 정의와 동시에 즉시 호출되는 함수

```jsx
(function () {
	var a = 3;
	var b = 5;
	return a * b;
}());
```

### 재귀 함수

함수가 자기 자신을 호출하는 것을 재귀호출이라 한다.

재귀 함수는 자신을 무한 재귀 호출한다. 따라서 재귀 함수 내에는 재귀 호출을 멈출 수 있는 **탈출 조건**을 반드시 만들어야 한다.

```jsx
// Computing Facotrial
// n! = 1 * 2 * ... * (n-1) * n
function factorial(n) {
	if (n <= 1) return 1;
	return n * factorial(n-1);
}

function factorial(n) {
	if (n <= 1) return 1;
	
	var res = n;
	while (--n) res *= n;
	return res;
}
```

### 중첩 함수

함수 내부에 정의된 함수를 중첩 함수 또는 내부 함수라 한다.

```jsx
function outer() {
	var x = 1;

	function inner() {
		var y = 2;
		console.log(x + y); // 3
	}
	
	inner();
}

outer();
```

### 콜백 함수

함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 콜백 함수라고 한다.
```jsx
function repeat(n, f){
	for (var i = 0; i < n; i++) {
		f(i);
	}
}

var logAll = function (i) {
	console.log(i);
};

repeat(5, logAll);  // 0 1 2 3 4

var logOdds = function (i) {
	if (i % 2) console.log(i);
};

repeat(5, logOdds);  // 1 3
```
