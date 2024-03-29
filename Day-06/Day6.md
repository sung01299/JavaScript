# JS Day06

# 생성자 함수에 의한 객체 생성

## Object 생성자 함수

new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다.

빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.

```jsx
const person = new Object();

person.name = 'Lee';
person.sayHello = function() {
	console.log('Hi! My name is ' + this.name);
};

console.log(person);  // {name: "Lee", sayHello: f}
person.sayHello();  // Hi! My name is Lee
```

## 생성자 함수

### 객체 리터럴에 의한 객체 생성 방식의 문제점

객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성한다.

따라서 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적이다.

### 생성자 함수에 의한 객체 생성 방식의 장점

프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

```jsx
function Circle(radius) {
	this.radius = radius;
	this.getDiameter = function() {
		return 2 * this.radius;
	};
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter);  // 10
console.log(circle2.getDiameter);  // 20
```

### 생성자 함수의 인스턴스 생성 과정

1. 인스턴스 생성과 this 바인딩
2. 인스턴스 초기화
3. 인스턴스 반환

### 내부 메서드 [[Call]]과 [[Construct]]

함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 [[Call]]이 호출되고 new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 [[Construct]]가 호출된다.

결론적으로 함수 객체는 callable이며서 constructor이거나 callable이면서 non-constructor다.

### constructor와 non-constructor의 구분

- constructor: 함수 선언문, 함수 표현식, 클래스 ⇒ 생성자 함수로서 호출할 수 있는 함수
- non-constructor: 메서드, 화살표 함수 ⇒ 생성자 함수로서 호출할 수 없는 함수

### new 연산자

new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다.

### new.target

new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다.

new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.

# 함수와 일급 객체

## 일급 객체

1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

함수는 위의 조건을 모두 만족하므로 일급 객체다.

## 함수 객체의 프로퍼티

### arguments 프로퍼티

함수 객체의 arguments 프로퍼티 값은 arguments 객체다. arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용된다.

### length 프로퍼티

함수 객체의 length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.

### name 프로퍼티

함수 객체의 name 프로퍼티는 함수 이름을 나타낸다.

### __**proto__ 접근자 프로퍼티**

__proto__ 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다.

### prototype 프로퍼티

prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor 만이 소유하는 프로퍼티다.

일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없다.

```jsx
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty('prototype');  // -> true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype');  // -> false
```

# 프로토타입

자바스크립트는 객체 기반의 프로그래밍 언어이며 **자바스크립트를 이루고 있는 거의 “모든 것”이 객체다.**

원시 타입의 값을 제외한 나머지 값들(함수, 배열, 정규 표현식 등)은 모두 객체다.

## 객체지향 프로그래밍

프로그램을 여러 개의 독립적 단위, 즉 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 말한다.

속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조를 **객체**라 한다.

```jsx
const circle = {
	radius: 5,
	getDiameter() {
		return 2 * this.radius;
	},
	getPerimeter() {
		return 2 * Math.Pi * this.radius;
	},
	getArea() {
		return Math.Pi * this.radius ** 2;
	},
};

console.log(circle);
// {radius: 5, getDiameter: f, getPerimeter: f, getArea: f}
console.log(circle.getDiameter());  // 10
console.log(circle.getPerimeter()); // 31.41592...
console.log(circle.getArea());      // 78.53981...
```

## 상속과 프로토타입

**상속**은 객체 지향 프로그래밍의 핵심 개념으로, 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.

자바스크립트는 프로토타입을 기반으로 상속을 구현한다.

```jsx
function Circle(radius) {
	this.radius = radius;
}

Circle.prototype.getArea = function() {
	return Math.PI * this.radius ** 2;
};

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea);  // true
console.log(circle1.getArea());  // 3.141592...
console.log(circle2.getArea());  // 12.56637...
```

## 프로토타입 객체

프로토타입 객체란 객체지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용된다.

### __proto__ 접근자 프로퍼티

모든 객체는 __proto__ 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 [[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.

```jsx
// __proto__ 접근자 프로퍼티는 상속을 통해 사용된다.
const person = { name: 'Lee'}
console.log(person.hasOwnProperty('__proto__'));  // false
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));
console.log({}.__proto__ === Object.prototype);  // true

// __proto__ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유
// 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해
const parent = {};
const child = {};

child.__proto__ = parent;
parent.__proto__ = child;  // TypeError: Cyclic __proto__ value

// __proto__ 접근자 프로퍼티를 코드 내에서 직접 사용하는것은 권장하지 않는다.
// obj는 프로토타입 체인의 종점이다. 따라서 Object.__proto__를 상속받을 수 없다.
const obj = Object.create(null);

// obj는 Object.__proto__를 상속받을 수 없다.
console.log(obj.__proto__);  // undefined

// 따라서 __proto__보다 Object.getPrototypeOf 메서드를 사용하는 편이 좋다.
console.log(Object.getPrototypeOf(obj));  // null

// 프로토타입을 교체하고 싶은 경우에는 Object.setPrototypeOf 메서드를 사용한다.
const obj = {};
const parent = { x : 1 };
Object.getPrototypeOf(obj);
Object.setPrototypeOf(obj, parent);  // obj.__proto__ = parent;

console.log(obj.x);  // 1
```

### 함수 객체의 prototype 프로퍼티

**함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.**

```jsx
(function () {}).hasOwnProperty('prototype');  // true
({}).hasOwnProperty('prototype');  // false
```

| 구분 | 소유 | 값 | 사용 주체 | 사용 목적 |
| --- | --- | --- | --- | --- |
| __proto__ 접근자 프로퍼티 | 모든 객체 | 프로토타입의 참조 | 모든 객체 | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용 |
| prototype 접근자 프로퍼티 | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체의 프로토타입을 할당하기 위해 사용 |

### 프로토타입의 constructor 프로퍼티와 생성자 함수

모든 프로토타입은 constructor 프로퍼티를 갖는다.

이 constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.

## 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

생성자 함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결된다.

리터럴 표기법에 의해 생성된 객체의 경우 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수는 없다.

**프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.**

## 프로토타입의 생성 시점

객체는 리터럴 표기법 또는 생성자 함수에 의해 생성되므로 결국 모든 객체는 생성자 함수와 연결되어 있다.

**프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.**

### 사용자 정의 생성자 함수와 프로토타입 생성 시점

생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

생성자 함수로서 호출할 수 없는 함수, 즉 non-constructor는 프로토타입이 생성되지 않는다.

### 빌트인 생성자 함수와 프로토타입 생성 시점

Object, String, Number, Function, Array, RegExp, Date, Promise 등과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.

## 객체 생성 방식과 프로토타입의 결정

### 객체 리터럴에 의해 생성된 객체의 프로토타입

```jsx
const obj = { x : 1 };

console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x'));    // true
```

### Object 생성자 함수에 의해 생성된 객체의 프로토타입

```jsx
const obj = new Object();
obj.x = 1;

console.log(obj.constructor === Object);  // true
console.log(obj.hasOwnProperty('x'));     // true
```

### 생성자 함수에 의해 생성된 객체의 프로토타입

```jsx
function Person(name) {
	this.name = name;
}

Person.prototype.sayHello = function () {
	console.log('Hi! My name is ${this.name}');
};

const me = new Person('Lee');
const you = new Person('Kim');

me.sayHello();  // Hi! My name is Lee
you.sayHello(); // Hi! My name is Kim
```

## 프로토타입 체인

객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 이를 프로토타입 체인이라 한다. 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.

Object.prototype을 프로토타입 체인의 종점 (end of prototype chain)이라 한다.

프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘이라고 할 수 있다.

스코프 체인은 식별자 검색을 위한 메커니즘이라고 할 수 있다.

스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는데 사용된다.

## 오버라이딩과 프로퍼티 섀도잉

오버라이딩: 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식이다.

프로퍼티 섀도잉: 상속 관계에 의해 프로퍼티가 가려지는 현상.

## 프로토타입의 교체

### 생성자 함수에 의한 프로토타입의 교체

```jsx
const Person = (function () {
	function Person(name) {
		this.name = name;
	}
	
	Person.prototype = {
		constructor: Person,
		sayHello() {
			console.log('Hi! My name is ${this.name}'};
		}
	};
	
	return Person;
}());

const me = new Person('Lee');

console.log(me.constructor === Person);  // true
console.log(me.constructor === Object);  // false
```

### 인스턴스에 의한 프로토타입의 교체

```jsx
function Person(name){
	this.name = name;
}

const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {
	sayHello() {
		console.log('Hi! My name is ${this.name}');
	}
};

Object.setPrototypeOf(me, parent);
// me.__proto__ = parent;

me.sayHello();  // Hi! My name is Lee
```

## instanceof 연산자

우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가되고, 그렇지 않은 경우에는 false로 평가된다.

생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.

## 직접 상속

### Object.create에 의한 직접 상속

첫 번째 매개변수: 생성할 객체의 프로토타입으로 지정할 객체

두 번째 매개변수: 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이루어진 객체

```jsx
obj = Object.create(Object.prototype, {
	x: { value: 1, writable: true, enumerable: true, configurable: true }
});
console.log(obj.x);  // 1
console.log(Object.getPrototypeOf(obj) === Object.prototype);  // true

const myProto = { x: 10 };
obj = Object.create(myProto);
console.log(obj.x);  // 10
console.log(Object.getPrototypeOf(obj) === myProto);  // true
```

### 객체 리터럴 내부에서 __proto__ 에 의한 직접 상속

```jsx
const myProto = { x: 10 };

const obj = {
	y: 20,
	__proto__: myProto
};

console.log(obj.x, obj.y);  // 10 20
console.log(Object.getPrototypeOf(obj) === myProto);  // true
```

## 정적 프로퍼티/메서드

정적 프로퍼티/메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드를 말한다.

## 프로퍼티 존재 확인

### in 연산자

```jsx
/**
* key: 프로퍼티 키를 나타내는 문자열
* object: 객체로 평가되는 표현식
*/
key in Object
```

```jsx
const person = {
	name: 'Lee',
	address: 'Seoul'
};

console.log('name' in person);  // true
console.log('address' in person);  // true
console.log('age' in person);  // false

// Reflect.has 메서드를 대신 사용할 수도 있다.
console.log(Reflect.has(person, 'name'));  // true
```

### Object.prototype.hasOwnProperty 메서드

```jsx
console.log(person.hasOwnProperty('name'));  // true
console.log(person.hasOwnProperty('age'));  // false
```

## 프로퍼티 열거

### for … in 문

for … in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 **[[Enumerable]]**의 값이 true인 프로퍼티를 순회하며 열거한다.

```jsx
// for ( 변수 선언문 in 객체 ) { … }
const person = {
	name: 'Lee',
	address: 'Seoul'
};

for (const key in person) {
	console.log(key + ': ' + person[key]);
}
// name: Lee
// address: Seoul
```

```jsx
const arr = [1, 2, 3];
arr.x = 10;

for (const i in arr) {
	console.log(arr[i]);  // 1 2 3 10
};

for (let i = 0; i < arr.length; i++) {
	console.log(arr[i]);  // 1 2 3
};

arr.forEach(v => console.log(v));  // 1 2 3

for (const value of arr) {
	console.log(value);  // 1 2 3
};
```

### Object.keys/values/entries 메서드

```jsx
// Object.keys 메서드는 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환한다.
const person = {
	name: 'Lee',
	address: 'Seoul',
	__proto__: { age: 20 }
};

console.log(Object.keys(person));  // ["name", "address"]

// Object.values 메서드는 객체 자신의 열거가능한 프로퍼티 값을 배열로 반환한다.
console.log(Object.values(person));  // ["Lee", "Seoul"]

// Object.entries 메서드는 객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환.
console.log(Object.entries(person));  // [["name", "Lee"], ["address", "Seoul"]]
Object.entreis(person).forEach(([key, value]) => console.log(key, value));
/*
name Lee
address Seoul
*/
```
