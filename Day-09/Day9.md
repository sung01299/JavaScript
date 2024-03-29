# JS Day09

# ES6 함수의 추가 기능

## 함수의 구분

ES6 이전의 모든 함수는 동일한 함수라도 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다.

```jsx
var foo = function(){
	return 1;
};

foo();  // 1
new foo();  // foo {}
var obj = {foo: foo};
obj.foo();  // 1
```

주의할 것은 ES6 이전에 일반적으로 메서드라고 부르던 객체에 바인딩된 함수도 callable이며 constructor라는 것이다.

```jsx
var obj = {
	x: 10;
	f: function() { return this.x; }
};

console.log(obj.f());  // 10
var bar = obj.f;
console.log(bar());  // undefined
console.log(new obj.f());  // f {}
```

| ES6 함수의 구분 | constructor | prototype | super | arguments |
| --- | --- | --- | --- | --- |
| 일반 함수 | O | O | X | O |
| 메서드 | X | X | O | O |
| 화살표 함수 | X | X | X | X |

## 메서드

ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.

```jsx
const obj = {
	x: 1,
	// foo는 메서드다.
	foo() { return this.x },
	// bar에 바인딩된 함수는 메서드가 아닌 일반 함수다.
	bar: function() { return this.x; }
};

console.log(obj.foo());  // 1
console.log(obj.bar());  // 1

// ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor다.
// 따라서 ES6 메서드는 생성자 함수로서 호출할 수 없다.
new obj.foo();  // TypeError: obj.foo is not a constructor
new obj.bar();  // bar {}

// ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부슬롯 [[HomeObject]]를 갖는다.
const base = {
	name: 'Lee';
	sayHi() {
		return `Hi! ${this.name}`;
	}
};

const derived = {
	__proto__: base,
	sayHi() {
		return `${super.sayHi()}. How are you doing?`;
	}
};

console.log(derived.sayHi());  // Hi! Lee. How are you doing?
```

이처럼 ES6 메서드는 본연의 기능(super)을 추가하고 의미적으로 맞지 않는 기능(constructor)은 제거했다.

## 화살표 함수

화살표 함수는 function 키워드 대신 화살표(⇒)를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다.

### 화살표 함수 정의

```jsx
// 함수 정의
const multiply = (x, y) => x * y;
multiply(2, 3);  // 6

// 매개변수 선언
// 매개변수가 여러 개 인경우 소괄호안에 매개변수를 선언한다.
const arrow = (x, y) => { ... };
// 매개변수가 한 개인 경우 소괄호를 생략할 수 있다.
const arrow = x => { ... };
// 매개변수가 없는 경우 소괄호를 생략할 수 없다.
const arrow = () => { ... };

// 함수 몸체 정의
// 함수 몸체가 하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호를 생략할 수 있다.
const power = x => x ** 2;
power(2);  // 4
// 함수 몸체를 감싸는 중괄호를 생략한 경우 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러가 발생한다.
const arrow = () => const x = 1;
// SyntaxError: Unexpected token 'const'
// 함수 몸체가 하나의 문으로 구성된다 해도 함수 몸체의 문이 표현식이 아닌 문이라면
// 중괄호를 생략할 수 없다.
const arrow = () => { const x = 1; };
// 객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호로 감싸 주어야 한다.
const create = (id, content) => ({id, content});
create(1, 'JavaScript');  // {id: 1, content: "JavaScript"}
// 함수 몸체가 여러 개의 문으로 구성된다면 함수 몸체를 감싸는 중괄호를 생략할 수 없다.
const sum = (a, b) => {
	const result = a + b;
	return result;
};
// 즉시 실행 함수로 사용할 수 있다.
const person = (name => ({
	sayHi() { return `Hi? My name is ${name}.`; }
}))('Lee');
```

### 화살표 함수와 일반 함수의 차이

1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.

```jsx
const Foo =() => {};
new Foo();  // TypeError: Foo is not a constructor
```

1. 중복된 매개변수 이름을 선언할 수 없다.

```jsx
// 일반 함수는 중복된 매개변수 이름을 선언해도 에러가 발생하지 않는다.
function normal(a, a){ return a + a; }
console.log(normal(1, 2));  // 4

// 화살표 함수에서는 중복된 매개변수 이름을 선언하면 에러가 발생한다.
const arrow = (a, a) => a + a;
// SyntaxError: Duplicate parameter name not allowed in this context
```

1. 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.
- 화살표 함수 내부에서 this, arguments, super, new.target을 참조하면 스코프 체인을 통해 상위 스코프의 this, arguments, super, new.target을 참조한다.

### this

화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다.

따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 참조한다. 이를 lexical this라 한다.

### super

화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다.

따라서 화살표 함수 내부에서 super를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조한다.

### arguments

화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다.

따라서 화살표 함수 내부에서 arguments를 참조하면 this와 마찬가지로 상위 스코프의 arguments를 참조한다.

## Rest 파라미터

### 기본 문법

Rest 파라미터(나머지 매개변수)는 매개변수 이름 앞에 세개의 점 … 을 붙여서 정의한 매개변수를 의미한다.

Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.

```jsx
function foo(... rest){
	console.log(rest);  // [1, 2, 3, 4, 5]
}
foo(1, 2, 3, 4, 5);

function foo(param, ... rest){
	console.log(param);  // 1
	console.log(rest);  // [2, 3, 4, 5]
}
foo(1, 2, 3, 4, 5);

function bar(param1, param2, ...rest){
	console.log(param1);  // 1
	console.log(param2);  // 2
	console.log(rest);  // [3, 4, 5]
}
bar(1, 2, 3, 4, 5);

// Rest 파라미터는 반드시 마지막 파라미터이어야 하고 단 하나만 선언할 수 있다.
function foo(... rest, param1, param2){}
// SyntaxError: Rest parameter must be last formal parameter
function foo(...rest1, ...rest2){}
// SyntaxError: Rest parameter must be last formal parameter
```

### Rest 파라미터와 arguments 객체

ES5에서 arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이므로, 배열 메서드를 사용하려면 Function.prototype.call이나 Function.prototype.apply 메서드를 사용해 arguments 객체를 배열로 변환해야 하는 번거로움이 있었다.

ES6에서는 rest 파라미터를 사용하여 인수 목록을 배열로 직접 전달받을 수 있다.

```jsx
function sum(... args){
	return args.reduce((pre, cur) => pre + cur, 0);
}
console.log(sum(1, 2, 3, 4, 5));  // 15
```

## 매개변수 기본값

ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다.

```jsx
function sum(x=0, y=0){
	return x + y;
}
console.log(sum(1, 2));  // 3
console.log(sum(1));  // 1

// Rest 파라미터에는 기본값을 지정할 수 없다.
function foo(...rest = []){
	console.log(rest);  // SyntaxError: Rest parameter may not have a default initializer
}

// 매개변수 기본값은 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티와
// arguments 객체에 아무런 영향을 주지 않는다.
function sum(x, y=0){
	console.log(arguments);
}
console.log(sum.length);  // 1
sum(1);  // Arguments {'0': 1 }
sum(1, 2);  // Arguments {'0': 1, '1': 2 }
```

# 배열

## 배열이란?

배열은 여러 개의 값을 순차적으로 나열한 자료구조다.

```jsx
const arr = ['apple', 'banana', 'orange'];
arr[0] // 'apple'
arr[1] // 'banana'
arr[2] // 'orange'
arr.length // 3
// for 문을 통해 순차적으로 요소에 접근할 수 있다.
for (let i = 0; i < arr.length; i++){
	console.log(arr[i]);  // 'apple', 'banana', 'orange'
}
typeof arr // object
```

## 자바스크립트 배열은 배열이 아니다.

자바스크립트의 배열은 지금까지 살펴본 자료구조에서 말하는 일반적인 의미의 배열과 다르다.

즉, 배열의 요소를 위한 각각의 메모리 공간은 동일한 크기를 갖지 않아도 되며, 연속적으로 이어져 있지 않을 수도 있다.

이처럼 배열의 요소가 연속적으로 이어져 있지 않는 배열을 희소 배열이라 한다.

## length 프로퍼티와 희소 배열

length 프로퍼티는 요소의 개수, 즉 배열의 길이를 나타내는 0 이상의 정수를 값으로 갖는다.

희소 배열은 length와 배열 요소의 개수가 일치 하지 않는다.

희소 배열의 length는 희소 배열의 실제 요소 개수보다 언제나 크다.

**자바스크립트는 문법적으로 희소 배열을 허용하지만 희소 배열은 사용하지 않는 것이 좋다.**

```jsx
const sparse = [, 2, , 3];
console.log(sparse.length) // 4
console.log(sparse). // [ <1 empty item>, 2, <1 empty item>, 3 ]
console.log(Object.getOwnPropertyDescriptors(sparse));
/* 
{
  '1': { value: 2, writable: true, enumerable: true, configurable: true },
  '3': { value: 3, writable: true, enumerable: true, configurable: true },
  length: { value: 4, writable: true, enumerable: false, configurable: false }
}
*/
```

## 배열 생성

### 배열 리터럴

가장 일반적이고 간편한 배열 생성 방식

```jsx
const arr = [1, 2, 3];
console.log(arr.length);  // 3
```

### Array 생성자 함수

```jsx
const arr = new Array(10);
console.log(arr);  // [empty * 10]
console.log(arr.length);  // 10

new Array();  // []
new Array(1, 2, 3);  // [1, 2, 3]
new Array({});  // [{}]
Array(1, 2, 3);  // [1, 2, 3]
```

### Array.of

```jsx
Array.of(1);  // [1]
Array.of(1, 2, 3);  // [1, 2, 3]
Array.of('string');  // ['string']
```

### Array.from

```jsx
// 유사 배열 객체를 변환하여 배열을 생성한다.
Array.from({length: 2, 0: 'a', 1: 'b'});  // ['a', 'b']
// 이터러블을 변환하여 배열을 생성한다.
Array.from('Hello');  // ['H', 'e', 'l', 'l', 'o']

Array.from({length: 3});  // [undefined, undefined, undefined]
Array.from({length: 3}, (_, i) => i);  // [0, 1, 2]
```

## 배열 요소의 참조

```jsx
const arr = [1, 2];
console.log(arr[0]);  // 1
console.log(arr[1]);  // 2
console.log(arr[2]);  // undefined
```

## 배열 요소의 추가와 갱신

존재하지 않는 인덱스를 사용해 값을 할당하면 새로운 요소가 추가된다.

```jsx
const arr = [0];
arr[1] = 1;
console.log(arr);  // [0, 1]
arr[100] = 100;
console.log(arr);  // [0, 1, empty * 98, 100]
// 이미 요소가 존재하는 요소에 값을 재할당하면 요소값이 갱신된다.
arr[1] = 10;
console.log(arr);  // [0, 10, empty * 98, 100]
```

## 배열 요소의 삭제

```jsx
const arr = [1, 2, 3];
delete arr[1];
console.log(arr);  // [1, empty, 3]

// 희소 배열을 만들지 않으면서 특정 요소를 완전히 삭제하려면 Array.prototype.splice 메서드를 사용한다.
const arr = [1, 2, 3];
arr.splice(1, 1);  // 삭제를 시작할 인덱스, 삭제할 요소 수
console.log(arr);  // [1, 3]
console.log(arr.length);  // 2
```

## 배열 메서드

배열에는 원본 배열을 직접 변경하는 메서드와 원본 배열을 변경하지 안고 새로운 배열을 생성하여 반환하는 메서드가 있다.

```jsx
const arr = [1];

// push 메서드는 원본 배열을 직접 변경한다.
arr.push(2);  // [1, 2]
// concat 메서드는 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환한다.
const result = arr.concat[3];
console.log(arr);  // [1, 2]
console.log(result);  // [1, 2, 3]
```

### Array.isArray

전달된 인수가 배열이면 true, 아니면 false를 반환한다.

```jsx
Array.isArray([]);  // true
Array.isArray([1, 2]);  // true
Array.isArray();  // false
Array.isArray(1);  // false
```

### Array.prototype.indexOf

indexOf 메서드는 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환한다.

```jsx
const arr = [1, 2, 2, 3];

// 배열에서 요소 2를 검색하여 첫 번째로 검색된 요소의 인덱스를 반환한다.
arr.indexOf(2);  // 1
// 배열에 요소 4가 없으므로 -1을 반환한다.
arr.indexOf(4);  // -1
// 두 번째 인수는 검색을 시작할 인덱스다. 두 번째 인수를 생략하면 처음부터 검색한다.
arr.indexOf(2, 2);  // 2

// Array.prototype.includes 메서드를 사용하여 배열에 특정 요소가 존재하는지 확인할수 있다.
if (!arr.includes(4)){
	arr.push(4);
}

console.log(arr);  // [1, 2, 2, 3, 4]
```

### Array.prototype.push

push 메서드는 인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가하고 변경된 length 프로퍼티 값을 반환한다.

```jsx
const arr = [1, 2];
let result = arr.push(3, 4);
console.log(result);  // 4
console.log(arr);  // [1, 2, 3, 4]

// push 메서드 보다는 ES6의 스프레드 문법을 사용하는 편이 좋다.
const arr = [1, 2];
const newArr = [...arr, 3];
console.log(newArr);  // [1, 2, 3]
```

### Array.prototype.pop

pop 메서드는 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환한다.

```jsx
const arr = [1, 2];
let result = arr.pop();
console.log(result);  // 2
console.log(arr);  // [1]

// push와 pop을 사용하여 스택을 구현할 수 있다.
const Stack = (function () {
	function Stack(array = []){
		if (!Array.isArray(array)){
			throw new TypeError(`${array} is not an array.`);
		}
		this.array = array;
	}
	Stack.prototype = {
		constructor: Stack.
		push(value) {
			return this.array.push(value);
		},
		pop(value) {
			return this.array.pop();
		},
		entries() {
			return [... this.array];
		}
	};
	return Stack
}());

const stack = new Stack([1, 2]);
console.log(stack.entries());  // [1, 2]

stack.push(3);
console.log(stack.entreis());  // [1, 2, 3]

stack.pop();
console.log(stack.entreis());  // [1, 2]
```

### Array.prototype.unshift

unshift 메서드는 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 length 프로퍼티 값을 반환한다.

```jsx
const arr = [1, 2];
let result = arr.unshift(3, 4);
console.log(result);  // 4
console.log(arr);  // [3, 4, 1, 2]

// unshift 메서드보다는 ES6의 스프레드 문법을 사용하는 편이 좋다.
const arr = [1, 2];
const newArr = [3, ... arr];
console.log(newArr);  // [3, 1, 2]
```

### Array.prototype.shift

shift 메서드는 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환한다.

```jsx
const arr = [1, 2];
let result = arr.shift();
console.log(result);  // 1
console.log(arr);  // [2]

// shift 메서드와 push 메서드를 사용하면 큐 Queue를 쉽게 구현할 수 있다.
const Queue = (function () {
	function Queue(array = []) {
		if (!Array.isArray(array)) {
			throw new TypeError(`${array} is not an array.`);
		}
		this.array = array;
	}
	Queue.prototype = {
		constructor: Queue,
		enqueue(value) {
			return this.array.push(value);
		},
		dequeue() {
			return this.array.shift();
		},
		entries() {
			return [ ... this.array];
		}
	};
	return Queue;
}());
```

### Array.prototype.concat

concat 메서드는 인수로 전달된 값들을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환한다.

```jsx
const arr1 = [1, 2];
const arr2 = [3, 4];
let result = arr1.concat(arr2);  // [1, 2, 3, 4]
result = arr1.concat(3);  // [1, 2, 3]
result = arr1.concat(arr2, 5);  // [1, 2, 3, 4, 5]
console.log(arr1);  // [1, 2]

// concat 메서드는 ES6의 스프레드 문법으로 대체할 수 있다.
result = [...[1,2], ...[3,4]];
console.log(result);  // [1, 2, 3, 4]
```

결론적으로 push/unshift 메서드와 concat 메서드를 사용하는 대신 ES6의 스프레드 문법을 일관성 있게 사용하는 것을 권장한다.

### Array.prototype.splice

원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거하는 경우 splice 메서드를 사용한다.

splice 메서드는 3개의 매개변수가 있으며 원본 배열을 직접 변경한다.

1. start: 원본 배열의 요소를 제거하기 시작할 인덱스다.
2. deleteCount: 원본 배열의 요소를 제거하기 시작할 인덱스인 start부터 제거할 요소의 개수다.
3. items: 제거한 위치에 삽입할 요소의 목록이다.

```jsx
const arr = [1, 2, 3, 4];
const result = arr.splice(1, 2, 20, 30);
console.log(result);  // [2, 3]
console.log(arr);  // [1, 20, 30, 4]

const result = arr.splice(1, 0, 100);
console.log(arr);  // [1, 100, 2, 3, 4]

const result = arr.splice(1, 2);
console.log(arr);  // [1, 4]

const result = arr.splice(1);
console.log(arr);  // [1]
```

### Array.prototype.slice

slice 메서드는 인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다.

slice 메서드는 2개의 매개변수를 갖는다.

1. start: 복사를 시작할 인덱스다.
2. end: 복사를 종료할 인덱스다.

```jsx
const arr = [1, 2, 3];

arr.slice(0, 1);  // [1]
arr.slice(1, 2);  // [2]
arr.slice(1);  // [2, 3]
arr.slice(-1);  // [3]
arr.slice(-2);  // [2, 3]
```

### Array.prototype.join

join 메서드는 원본 배열의 모든 요소를 문자열로 변환한 후, 인수로 전달받은 문자열, 즉 구분자로 연결한 문자열을 반환한다.

```jsx
const arr = [1, 2, 3, 4];
arr.join();  // '1,2,3,4';
arr.join('');  // '1234';
arr.join(':');  // '1:2:3:4'
```

### Array.prototype.reverse

reverse 메서드는 원본 배열의 순서를 반대로 뒤집는다.

```jsx
const arr = [1, 2, 3];
const result = arr.reverse();
console.log(arr);  // [3, 2, 1]
console.log(result);  // [3, 2, 1]
```

### Array.prototype.fill

fill 메서드는 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다.

```jsx
const arr = [1, 2, 3];

arr.fill(0);  // [0, 0, 0]
// 0을 배열의 인덱스 1부터 끝까지 요소로 채운다.
arr.fill(0, 1);  // [1, 0, 0]
// 0을 인덱스 1부터 3이전가지 요소로 채운다.
const arr = [1, 2, 3, 4, 5];
arr.fill(0, 1, 3);  // [1, 0, 0, 4, 5]
```

### Array.prototype.includes

includes 메서드는 배열 내에 특정 요소가 포함되어 있는지 확인하여 true또는 false를 반환한다.

```jsx
const arr = [1, 2, 3];

arr.includes(2);  // true
arr.includes(100);  // false
// 배열에 요소 1이 포함되어 있는지 인덱스 1부터 확인한다.
arr.includes(1, 1);  // false
arr.includes(3, -1); // true
```

### Array.prototype.flat

flat 메서드는 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화 한다.

```jsx
[1, [2, 3, 4, 5]].flat();  // [1, 2, 3, 4, 5]
[1, [2, [3, [4]]]].flat();  // [1, 2, [3, [4]]]
[1, [2, [3, [4]]]].flat(2);  // [1, 2, 3, [4]]
[1, [2, [3, [4]]]].flat(Infinity);  // [1, 2, 3, 4]
```

## 배열 고차 함수

고차 함수는 함수를 인수로 전달받거나 함수를 반환하는 함수를 말한다.

### Array.prototype.sort

sort 메서드는 배열의 요소를 정렬한다.

```jsx
const fruits = ['Banana', 'Orange', 'Apple'];
fruits.sort(); // ['Apple', 'Banana', 'Orange']
fruits.reverse();  // ['Orange', 'Banana', 'Apple']

// 숫자 요소를 정렬할 때는 sort 메서드에 정렬 순서를 정의하는 비교 함수를 인수로 전달해야 한다.
const points = [40, 100, 1, 5, 2, 25, 10];
points.sort((a, b) => a - b);
console.log(points);  // [1, 2, 5, 10, 25, 40, 100]
points.sort((a, b) => b - a);
console.log(points);  // [100, 40, 25, 10, 5, 2, 1]
```

### Array.prototype.forEach

forEach 메서드는 for 문을 대체할 수 있는 고차 함수다.

forEach 메서드는 자신의 내부에서 반복문을 실행한다.

```jsx
const numbers = [1, 2, 3];
const pows = [];

numbers.forEach(item => pows.push(item ** 2));
console.log(pows);  // [1, 4, 9]

class Numbers {
	numberArray = [];
	multiply(arr) {
		arr.forEach(item => this.numberArray.push(item * item));
	}
}

const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
console.log(numbers.numberArray);  // [1, 4, 9]
```

### Array.prototype.map

map 메서드는 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다

**그리고 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.**

map 메서드를 호출한 배열과 map 메서드가 생성하여 반환한 배열은 1:1 매핑한다.

```jsx
const numbers = [1, 4, 9];
const roots = numbers.map(item => Math.sqrt(item));
console.log(roots);  // [1, 2, 3]

class Prefixer{
	constructor(prefix){
		this.prefix = prefix;
	}
	add(arr){
		return arr.map(item => this.prefix + item);
	}
}
const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user']));
// ['-webkit-transition', '-webkit-user']
```

### Array.prototype.filter

filter 메서드는 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.

**그리고 콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환한다.**

filter 메서드가 생성하여 반환한 새로운 배열의 length 프로퍼티 값은 filter 메서드를 호출한 배열의 length 프로퍼티 값과 같거나 작다.

```jsx
const numbers = [1, 2, 3, 4, 5];
const odds = numbers.filter(item => item % 2);
console.log(odds);  // [1, 3, 5]
```

### Array.prototype.reduce

콜백 함수의 반환값을 다음 순회 시에 콜백 함수의 첫 번째 인수로 전달하면서 콜백 함수를 호출하여 하나의 결과값을 만들어 반환한다.

```jsx
const sum = [1, 2, 3, 4].reduce((accumulator, currentValue, index, array) =>
accumulator + currentValue, 0);
console.log(sum);  // 10

// 평균 구하기
const values = [1, 2, 3, 4, 5, 6];
const average = values.reduce((acc, curr, i, { length }) => {
	return i === length - 1 ? (acc + curr) / length : acc + curr;
}, 0);
console.log(average);  // 3.5

// 최대값 구하기
const values = [1, 2, 3, 4, 5];
const max = values.reduce((acc, curr) => (acc > cur ? acc : cur), 0);
console.log(max);  // 5

const max = Math.max(...values);  // 5

// 요소의 중복 횟수 구하기
const fruits = ['banana', 'apple', 'orange', 'orange', 'apple'];
const count = fruits.reduce((acc, cur) => {
	acc[cur] = (acc[cur] || 0) + 1;
	return acc;
}, {});
console.log(count);  // {banana: 1, apple: 2, orange: 2}

// 중첩 배열 평탄화
const values = [1, [2, 3], 4, [5, 6]];
const flatten = values.reduce((acc, cur) => acc.concat(cur), []);
console.log(flatten);  // [1, 2, 3, 4, 5, 6]

// 중복 요소 제거
const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];
const result = values.reduce(
	(unique, val, i, _values) =>
	_values.indexOf(val) === i ? [...unique, val] : unique, []);
console.log(result);  // [1, 2, 3, 5, 4]
```

### Array.prototype.some

some 메서드는 콜백 함수의 반환값이 단 한번이라도 참이면 true, 모두 거짓이면 false를 반환한다.

```jsx
[5, 10, 15].some(item => item > 10);  // true
[5, 10, 15].some(item => item < 0);  // false
['apple', 'banana', 'mango'].some(item => item === 'banana');  // true
[].some(item => item > 3);  // false
```

### Array.prototype.every

every 메서드는 콜백 함수의 반환값이 모두 참이면 true, 단 한 번이라도 거짓이면 false를 반환한다.

```jsx
[5, 10, 15].every(item => item > 3);  // true
[5, 10, 15].every(item => item > 10); // false
[].every(item => item > 3);  // true
```

### Array.prototype.find

find 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소를 반환한다.

콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 undefined를 반환한다.

```jsx
const users = [
	{ id: 1, name: 'Lee' },
	{ id: 2, name: 'Kim' },
	{ id: 3, name: 'Choi' },
	{ id: 4, name: 'Park' }
];
users.find(user => user.id === 2);  // {id: 2, name: 'Kim'}
```

### Array.prototype.findIndex

findIndex 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소의 인덱스를 반환한다.

콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 -1을 반환한다.

```jsx
const users = [
	{ id: 1, name: 'Lee' },
	{ id: 2, name: 'Kim' },
	{ id: 3, name: 'Choi' },
	{ id: 4, name: 'Park' }
];

users.findIndex(user => user.id === 2);  // 1
users.findIndex(user => user.name === 'Park');  // 3
function predicate(key, value){
	return item => item[key] === value;
}
users.findIndex(predicate('id', 2));  // 1
users.findIndex(predicate('name', 'Park'));  // 3
```

### Array.prototype.flatMap

flatMap 메서드는 map 메서드를 통해 새로운 배열을 평탄화한다.

즉, map 메서드와 flat 메서드를 순차적으로 실행하는 효과가 있다.

```jsx
const arr = ['hello', 'world'];
arr.map(x => x.split('')).flat();
arr.flatMap(x => x.split(''));
```
