# JS Day08

# 클로저

클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.

```jsx
const x = 1;

function outerFunc() {
	const x = 10;
	function innerFunc() {
		console.log(x);  // 10
	}
	
	innerFunc();
}

outerFunct();

// 중첩 함수 innerFunc의 상위 스코프는 외부 함수 outerFunc의 스코프다.
// 따라서 중첩 함수 innerFunc 내부에서 자신을 포함하고 있는 외부 함수 outerFunc의 x에 접근 가능.
```

## 렉시컬 스코프

자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정한다.

**렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 위해 결정된다.**

## 함수 객체의 내부 슬롯 [[Environment]]

함수는 자신의 내부 슬롯 [[Environment]]에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장한다.

함수 객체의 내부 슬롯 [[Environment]]에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프다.

또한 자신이 호출되었을 때 생성될 함수 렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”에 저장될 참조값이다.

함수 객체는 내부 슬롯 [[Environment]]에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억한다.

## 클로저와 렉시컬 환경

```jsx
const x = 1;

function outer() {
	const x = 10;
	const inner = function() { console.log(x); };
	return inner;
}

const innerFunc = outer();
innerFunc();  // 10
```

외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다.

이러한 중첩 함수를 **클로저**라고 부른다.

외부 함수보다 더 오래 생존한 중첩 함수는 외부 함수의 생존 여부와 상관없이 자신이 정의된 위치에 의해 결정된 상위 스코프를 기억한다. ([[Environment]]에 저장되어 있음)

**클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.**

클로저에 의해 참조되는 상위 스코프의 변수를 **자유 변수**라고 부른다.

## 클로저의 활용

**클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다.**

상태가 의도치 않게 변경되지 않도록 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용한다.

```jsx
const increase = (function () {
	let num = 0;
	return function () {
		return ++num;
	};
}());

console.log(increase());  // 1
console.log(increase());  // 2
console.log(increase());  // 3

// 카운드 상태를 감소시킬 수도 있도록 발전 시켜보자
const counter = (function () {
	let num = 0;
	return {
		increase() {
			return ++num;
		},
		decrease() {
			return num > 0 ? --num : 0;
		}
	};
}());

console.log(counter.increase());  // 1
console.log(counter.increase());  // 2

console.log(counter.decrease());  // 1
console.log(counter.decrease());  // 0

// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
const counter = (function () {
	let counter = 0;
	return function(aux){
		counter = aux(counter);
		return counter;
	};
}());

function increase(n){
	return ++n;
}

function decrease(n){
	return --n;
}
```

## 캡슐화와 정보 은닉

캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.

캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라 한다.

## 자주 발생하는 실수

```jsx
// for 문의 변수 선언문에서 var 키워드로 선언한 i 변수는 블록 레벨 스코프가 아닌 함수 레벨 스코프를
// 갖기 때문에 전역 변수다. 전역 변수 i에는 0, 1, 2가 순차적으로 할당된다. 따라서 funcs 배열의
// 요소로 추가한 함수를 호출하면 전역 변수 i를 참조하여 i의 값 3이 출력된다.
var funcs = [];

for (var i = 0; i < 3; i++){
	funcs[i] = function () { return i; };
}

for (var j = 0; j < funcs.length; j++){
	console.log(funcs[j]());  // 3 3 3
}

// 클로저를 사용해 바르게 동작하는 코드로 만들어보자
var funcs = [];

for (var i = 0; i < 3; i++){
	funcs[i] = (function (id) {
		return function () {
			return id;
		};
	}(i));
}

for (var j = 0; j < funcs.length; j++) {
	console.log(funcs[j]());
}

// let 키워드를 사용하면 이 같은 번거로움이 깔끔하게 해결된다.
const funcs = [];

for (let i = 0; i < 3; i++) {
	funcs[i] = function() { return i; };
}

for (let i = 0; i < funcs.length; i++) {
	console.log(funcs[i]());  // 0 1 2
}
// for 문의 변수 선언문에서 let 키워드로 선언한 변수를 사용하면 for 문의 코드 블록이 반복 
// 실행될 때마다 for 문 코드 블록의 새로운 렉시컬 환경이 생성된다.

// 함수형 프로그래밍 기법인 고차 함수를 사용하는 방법이 있다.
const funcs = Array.from(new Array(3), (_, i) => () => i);  // (3) [f,f,f]
funcs.forEach(f => console.log(f()));  // 0 1 2
```

# 클래스

자바스크립트는 프로토타입 기반 객체지향 언어이다.

프로토타입 기반 객체지향 언어는 클래스가 필요 없는 객체지향 프로그래밍 언어다.

ES5에서는 클래스 없이도 다음과 같이 생성자 함수와 프로토타입을 통해 객체지향 언어의 상속을 구현할 수 있다.

```jsx
var Person = (function () {
	function Person(name) {
		this.name = name;
	}
	
	Person.prototype.sayHi = function () {
		console.log('Hi! My name is ' + this.name);
	};
	
	return Person;
}());

var me = new Person('Lee');
me.sayHi();  // Hi! My name is Lee
```

ES6에서 도입된 클래스는 클래스 기반 객체지향 프로그래밍 언어와 매우 흡사한 새로운 객체 생성 메커니즘을 제시한다.

클래스와 생성자 함수의 차이:

1. 클래스는 new 연산자 없이 호출하면 에러가 발생한다. 하지만 생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출된다.
2. 클래스는 상속을 지원하는 extends와 super 키워드를 제공한다. 하지만 생성자 함수는 지원하지 않는다.
3. 클래스는 호이스팅이 발생하지 않는 것처럼 동작한다. 하지만 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.
4. 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 strict mode를 해제할 수 없다. 하지만 생성자 함수는 암묵적으로 strict mode가 지정되지 않는다.
5. 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다.

**클래스를 새로운 객체 생성 메커니즘으로 보는 것이 좀 더 합당하다.**

## 클래스 정의

```jsx
class Person {}
const Person = class {};
const Person = class MyClass {};

// 클래스 몸체에는 0개 이상의 메서드만 정의할 수 있다. constructor, 프로토타입 메서드, 정적
// 메서드를 정의할 수 있다.
class Person {
	// 생성자
	constructor(name) {
		this.name = name;
	}
	
	// 프로토타입 메서드
	sayHi() {
		console.log('Hi! My name is ${this.name}');
	}

	// 정적 메서드
	static sayHello() {
		console.log('Hello!');
	}
}
// 인스턴스 생성
const me = new Person('Lee');

console.log(me.name);  // Lee
me.sayHi();  // Hi! My name is Lee
Person.sayHello()  // Hello!
```

## 클래스 호이스팅

클래스 선언문도 변수 선언, 함수 정의와 마찬가지로 호이스팅이 발생한다.

단, 클래스는 let, const 키워드로 선언한 변수처럼 호이스팅한다.

일시적 사각지대에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작한다.

## 인스턴스 생성

클래스는 생성자 함수이며 반드시 new 연산자와 함께 호출되어 인스턴스를 생성한다.

```jsx
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me);  // Person {}
const me = Person();
// TypeError: Class constructor Foo cannot be invoked without 'new'
```

## 메서드

### constructor

constructor는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드다.

constructor는 이름을 변경할 수 없다.

```jsx
class Person {
	constructor(name) {
		this.name = name;
	}
}

class Person {
	// constructor는 생략하면 아래와 같이 빈 constructor가 암묵적으로 정의된다.
	constructor() {}
}

// 프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 constructor 내부에서 this에 인스턴스
// 프로퍼티를 추가한다.
class Person {
	constructor () {
		this.name = 'Lee';
		this.address = 'Seoul';
	}
}

const me = new Person();
console.log(me);  // Person {name: "Lee", address: "Seoul"}
```

### 프로토타입 메서드

생성자 함수를 사용하여 인스턴스를 생성하는 경우 프로토타입 메서드를 생성하기 위해서는 다음과 같이 명시적으로 프로토타입에 메서드를 추가해야 한다.

```jsx
// 생성자 함수
function Person(name) {
	this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHi = function() {
	console.log('Hi! My name is ${this.name}');
};

const me = new Person('Lee');
me.sayHi();  // Hi! My name is Lee
```

클래스 몸체에서 정의한 메서드는 생성자 함수에 의한 객체 생성 방식과는 다르게 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다.

```jsx
class Person {
	// 생성자
	constructor(name){
		this.name = name;
	}
	// 프로토타입 메서드
	sayHi(){
		console.log('Hi! My name is ${this.name});
	}
}

const me = new Person('Lee');
me.sayHi();  // Hi! My name is Lee	
```

### 정적 메서드

정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다.

생성자 함수의 경우 정적 메서드를 생성하기 위해서는 다음과 같이 명시적으로 생성자 함수에 메서드를 추가해야 한다.

```jsx
// 생성자 함수
function Person(name) {
	this.name = name;
}
// 정적 메서드
Person.sayHi = function() {
	console.log('Hi!');
};
// 정적 메서드 호출
Person.sayHi();  // Hi!
```

클래스에서는 메서드에 static 키워드를 붙이면 정적 메서드(클래스 메서드)가 된다.

```jsx
class Person {
	// 생성자
	constructor(name) {
	this.name = name;
	}

	// 정적 메서드
	static sayHi(){
		console.log('Hi!');
	}
}

Person.sayHi();  // Hi!
```

### 정적 메서드와 프로토타입 메서드의 차이

1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르다.
2. 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

### 클래스에서 정의한 메서드의 특징

1. function 키워드를 생략한 메서드 축약 표현을 사용한다.
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없다.
3. 암묵적으로 strict mode로 실행된다.
4. for … in 문이나 Object.keys 메서드 등으로 열거할 수 없다. 즉, 프로퍼티의 열거 가능 여부를 나타내며, 불리언 값을 갖는 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다.
5. 내부 메서드 [[Construct]]를 갖지 않는 non-constructor다. 따라서 new 연산자와 함께 호출할 수 없다.

## 클래스의 인스턴스 생성 과정

1. 인스턴스 생성과 this 바인딩
2. 인스턴스 초기화
3. 인스턴스 반환

## 프로퍼티

### 인스턴스 프로퍼티

인스턴스 프로퍼티는 constructor 내부에서 정의해야 한다.

```jsx
class Person {
	constructor(name) {
		// 인스턴스 프로퍼티
		this.name = name;  // name 프로퍼티는 public하다.
	}
}

const me = new Person('Lee');
console.log(me);  // Person {name: "Lee"}
console.log(me.name);  // Lee
```

### 접근자 프로퍼티

접근자 프로퍼티는 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다.

```jsx
const person = {
	// 데이터 프로퍼티
	firstName: 'Ungmo',
	lastName: 'Lee',

	// fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
	// getter 함수
	get fullName(){
		return `${this.firstName} ${this.lastName}`;
	},

	// setter 함수
	set fullName(name) {
		// 배열 디스트럭처링 할당
		[this.firstName, this.lastName] = name.split(' ');
	}
};

// 데이터 프로퍼티를 통한 프로퍼티 값을 참조
console.log(`${person.firstName} ${person.lastName}`);  // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값을 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
person.fullName = 'Heegun Lee';
console.log(person);  // {firstname: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(person.fullName);  // Heegun Lee

// fullName은 접근자 프로퍼티다.
// 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
console.log(Object.getOwnPropertyDescriptor(person, 'fullName'));
// {get: f, set: f, enumerable: true, configurable: true}
```

위 예제의 객체 리터럴을 클래스로 표현하면 다음과 같다.

```jsx
class Person {
	constructor(firstName, lastName){
		this.firstName = firstName;
		this.lastName = lastName;
	}

	get fullName(){
		return `${this.firstName} ${this.lastName}`;
	}

	set fullName(name){
		[this.firstName, this.lastName] = name.split(' ');
	}
}

const me = new Person('Ungmo', 'Lee');
```

접근자 프로퍼티는 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수, 즉 getter함수와 setter 함수로 구성되어 있다.

getter는 인스턴스 프로퍼티에 접근할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용한다.

setter는 인스턴스 프로퍼티에 값을 할당할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용한다.

### 클래스 필드 정의 제안

클래스 필드는 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어다.

```jsx
class Person {
	// 클래스 필드에 문자열을 할당
	name = 'Lee';
	// 클래스 필드에 함수를 할당
	getName = function () {
		return this.name;
	}
	// 화살표 함수로 정의할 수도 있다.
	// getName = () => this.name;
}

const me = new Person();
console.log(me);  // Person {name: "Lee", getName: f}
console.log(me.getName());  // Lee
```

### private 필드 정의 제안

인스턴스 프로퍼티는 인스턴스를 통해 클래스 외부에서 언제나 참조할 수 있다. 즉, 언제나 public이다.

```jsx
class Person {
	constructor(name) {
		this.name = name;  // 인스턴스 프로퍼티는 기본적으로 public하다.
	}
}

const me = new Person('Lee');
console.log(me.name);  // Lee

// 클래스 필드 정의 제안을 사용하더라도 클래스 필드는 기본적으로 public하기 때문에 외부에 그대로
// 노출된다.
class Person {
	name = 'Lee';  // 클래스 필드도 기본적으로 public하다.
}
// 인스턴스 생성
const me = new Person();
console.log(me.name);  // Lee
```

private 필드의 선두에는 #을 붙여준다. private 필드를 참조할 때도 #을 붙어주어야 한다.

```jsx
class Person {
	// private 필드 정의
	#name = '';

	constructor(name){
		// private 필드 참조
		this.#name = name;
	}
}
const me = new Person('Lee');
// private 필드 #name은 클래스 외부에서 참조할 수 없다.
console.log(me.#name);
// SyntaxError: Private field '#name' must be declared in an enclosing class
```

| 접근 가능성 | public | private |
| --- | --- | --- |
| 클래스 내부 | Yes | Yes |
| 자식 클래스 내부 | Yes | No |
| 클래스 인스턴스를 통한 접근 | Yes | No |

클래스 외부에서 private 필드에 직접 접근할 수 있는 방법은 없다.

하지만 접근자 프로퍼티를 통해 간접적으로 접근하는 방법은 유효하다.

```jsx
class Person {
	// private 필드 정의
	#name = '';
	constructor(name){
		this.#name = name;
	}
	// name은 접근자 프로퍼티다
	get name() {
		// private 필드를 참조하여 trim한 다음 반환한다.
		return this.#name.trim();
	}
}

const me = new Person('Lee');
console.log(me.name);  // Lee
```

### static 필드 정의 제안

```jsx
class MyMath {
	// static public 필드 정의
	static PI = 22 / 7;
	// static private 필드 정의
	static #num = 10;
	// static 메서드
	static increment() {
		return ++MyMath.#num;
	}
}

console.log(MyMath.PI);  // 3.142857...
console.log(MyMath.increment());  // 11
```

## 상속에 의한 클래스 확장

### 클래스 상속과 생성자 함수 상속

프로토타입 기반 상속은 프로토타입 체인을 통해 다른 객체의 자산을 상속 받는 개념이지만

**상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것이다.**

상속에 의한 클래스 확장은 코드 재사용 관점에서 매우 유용하다.

```jsx
class Animal {
	constructor(age, weight) {
		this.age = age;
		this.weight = weight;
	}
	eat() { return 'eat'; }
	move() { return 'move'; }
}

// 상속을 통해 Animal 클래스를 확장한 Bird 클래스
class Bird extends Animal {
	fly() { return 'fly'; }
}
const bird = new Bird(1, 5);

console.log(bird);  // Bird {age: 1, weight: 5}
console.log(bird.eat());  // eat
console.log(bird.move());  // move
console.log(bird.fly());  // fly
```

### extends 키워드

상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받을 클래스를 정의한다.

extends 키워드의 역할은 수퍼클래스와 서브클래스 간의 상속 관계를 설정하는 것이다.

```jsx
// 수퍼(베이스/부모) 클래스
class Base {}

// 서브(파생/자식) 클래스
class Derived extends Base {}
```

### 동적 상속

extends 키워드는 클래스뿐만 아니라 생성자 함수를 상속받아 클래스를 확장할 수도 있다. 단, extends 키워드 앞에는 반드시 클래스가 와야 한다.

```jsx
// 생성자 함수
function Base(a) {
	this.a = a;
}
// 생성자 함수를 상속받는 서브클래스
class Derived extends Base {}

const derived = new Derived(1);
console.log(derived);  // Derived {a: 1}

// extends 키워드 다음에는 클래스뿐만이 아니라 [[Construct]] 내부 메서드를 갖는 함수 객체로
// 평가될 수있는 모든 표현식을 사용할 수 있다. 이를 통해 동적으로 상속받을 대상을 결정한다.
let condition = true;

class Derived extends (condition ? Base1 : Base2) {}
```

### 서브클래스의 constructor

```jsx
// 클래스에서 constructor를 생략하면 클래스에 다음과 같이 비어있는 constructor가 암묵적으로
// 정의된다.
constructor() {}

// 서브클래스에서 constructor를 생략하면 클래스에 다음과 같은 constructor가 암묵적으로
// 정의된다. args는 new 연산자와 함께 클래스를 호출할 때 전달한 인수의 리스트다.
constructor(...args) { super(...args);}
// super()는 수퍼클래스의 constructor를 호출하여 인스턴스를 생성한다.
```

### super 키워드

super 키워드는 함수처럼 호출할 수도 있고 this와 같이 식별자처럼 참조할 수 있는 특수한 키워드다.

- super를 호출하면 수퍼클래스의 constructor를 호출한다.
- super를 참조하면 수퍼클래스의 메서드를 호출할 수있다.

### super 호출

super를 호출하면 수퍼클래스의 constructor를 호출한다.

```jsx
class Base {
	constructor(a, b){
		this.a = a;
		this.b = b;
	}
}

class Derived extends Base {}
const derived = new Derived(1, 2);
console.log(derived);  // Derived {a: 1, b: 2}

class Derived extends Base {
	constructor(a, b, c){
		super(a, b);
		this.c = c;
	}
}

const derived = new Derived(1, 2, 3);
console.log(derived);  // Derived {a: 1, b: 2, c: 3}
```

```jsx
// super를 호출할 때 주의할 사항:
// 1. 서브클래스에서 constructor를 생략하지 않는 경우 서브클래스의 constructor에서는 반드시
// super를 호출해야 한다.
class Base {}
class Derived extends Base{
	constructor(){
		// ReferenceError: Must call super constructor in derived class before ...
		console.log('constructor call');
	}
}

// 2. 서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없다.
class Base {}
class Derived extends Base{
	constructor(){
		// ReferenceError: Must call super constructor in derived class before ...
		this.a = 1;
		super();
	}
}

// 3. super는 반드시 서브클래스의 constructor에서만 호출한다.
class Base {
	constructor() {
		super();  // SyntaxError: 'super' keyword unexpected here
	}
}

function Foo() {
	super();  // SyntaxError: 'super' keyword unexpected here
}
```

### super 참조

메서드 내에서 super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.

```jsx
// 1. 서브클래스의 프로토타입 메서드 내에서 super.sayHi는 수퍼클래스의 프로토타입 메서드
// sayHi를 가리킨다.
class Base {
	constructor(name){
		this.name = name;
	}
	sayHi() {
		return `Hi! ${this.name}`;
	}
}

class Derived extends Base{
	sayHi() {
		return `${super.sayHi()}. how are you doing?`;
	}
}

const derived = new Derived('Lee');
console.log(derived.sayHi());  // Hi! Lee. How are you doing?

// 2. 서브클래스의 정적 메서드 내에서 super.sayHi는 수퍼클래스의 정적 메서드 sayHi를 가리킨다.
class Base {
	static sayHi() {
		return 'Hi!';
	}
}

class Derived extends Base{
	static sayHi(){
		return `${super.sayHi()} how are you doing?`;
	}
}

console.log(Derived.sayHi());  // Hi! how are you doing?
```

### 상속 클래스의 인스턴스 생성 과정

```jsx
class Rectangle {
	constructor(width, height) {
		this.width = width;
		this.height = height;
	}
	getArea() {
		return this.width * this.height;
	}
	toString() {
		return `width = ${this.width}, height = ${this.height}`;
	}
}

class ColorRectangle extends Rectangle {
	constructor(width, height, color) {
		super(width, height);
		this.color = color;
	}
	toString() {
		return super.toString() + `, color = ${this.color}`;
	}
}

const colorRectangle = new ColorRectangle(2, 4, 'red');
console.log(colorRectangle);  // ColorRectangle {width: 2, height: 4, color: "red"}

console.log(colorRectangle.getArea());  // 8
console.log(colorRectangle.toString());  // width = 2, height = 4, color = red
```

### 표준 빌트인 생성자 함수 확장

String, Number, Array 같은 표준 빌트인 객체도 extends 키워드를 사용하여 확장할 수 있다.

```jsx
class MyArray extends Array {
	uniq() {
		return this.filter((v, i, self) => self.indexOf(v) === i);
	}
	average() {
		return this.reduce((pre, cur) => pre + cur, 0) / this.length;
	}
}

const myArray = new MyArray(1, 1, 2, 3);
console.log(myArray);  // MyArray(4) [1, 1, 2, 3]
console.log(myArray.uniq());  // MyArray(3) [1, 2, 3]
console.log(myArray.average());  // 1.75
```
