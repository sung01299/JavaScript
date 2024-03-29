# JS Day07

# Strict mode

Strict mode는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

## strict mode의 적용

```jsx
'use strict';

function foo() {
	x = 10;  // ReferenceError: x is not defined
}
foo();

function() {
	'use strict';
	x = 10;  // ReferenceError: x is not defined
}
foo();
```

- 전역에 strict mode를 적용하는 것은 피하자
- 함수 단위로 strict mode를 적용하는 것도 피하자

## strict mode가 발생시키는 에러

### 암묵적 전역

```jsx
// 선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.
(function () {
	'use strict';
	x = 1;
	console.log(x);  // ReferenceError: x is not defined
}());
```

### 변수, 함수, 매개변수의 삭제

```jsx
// delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생한다.
(function () {
	'use strict';
	var x = 1;
	delete x;  // SyntaxError: Delete of an unqualified identifier in strict mode.
	
	function foo(a) {
		delete a;  // SyntaxError: Delete of an unqualified ... 
	}
	delete foo;  // SyntaxError: Delete of an unqualified ...
}());
```

### 매개변수 이름의 중복

```jsx
// 중복된 매개변수 이름을 사용하면 SyntaxError가 발생한다.
(function () {
	'use strict';

	// SyntaxError: Duplicate parameter name not allowed in this context
	function foo(x, x) {
		return x + x;
	}
	console.log(foo(1, 2));
}());
```

### with 문의 사용

```jsx
// with 문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서
// 코드가 간단해지는 효과가 있지만 성능과 가독성이 나빠지는 문제가 있다.
// 따라서 with 문은 사용하지 않는 것이 좋다.
(function () {
	'use strict';

	// SyntaxError: Strict mode code may not include a with statement
	with({ x: 1 }) {
		console.log(x);
	}
}());
```

## strict mode 적용에 의한 변화

### 일반 함수의 this

strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩 된다. 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문이다.

### arguments 객체

strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

# 빌트인 객체

## 자바스크립트 객체의 분류

- 표준 빌트인 객체
- 호스트 객체
- 사용자 정의 객체

## 표준 빌트인 객체

```jsx
const strObj = new String('Lee');  // String {"Lee"}
console.log(typeof strObj);  // object

const numObj = new Number(123);  // Number {123}
console.log(typeof numObj);  // object

... Boolean, Symbol, Function, Array, RegExp, Date, ...
```

## 원시값과 래퍼 객체

문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체 (wrapper object)라 한다.

```jsx
const str = 'hi';

console.log(str.length);  // 2
console.log(str.toUpperCase());  // HI

console.log(typeof str);  // string
```

## 전역 객체

전역 객체는 코드가 실행되기 이전 단계에서 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않은 최상위 객체이다.

브라우저 환경에서는 window(또는 self, this, frames), Node.js 환경에서는 global이 전역 객체를 가리킨다.

- 개발자가 의도적으로 생성할 수 없다.
- 프로퍼티를 참조할때 window(또는 global)를 생략할 수 있다.
- 모든 표준 빌트인 객체(Object, String, Number, … )를 프로퍼티로 가지고 있다.
- 자바스크립트 실행 환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다.
- var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.
- let이나 const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
- 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window를 공유한다.

### 빌트인 전역 프로퍼티

전역 객체의 프로퍼티를 의미한다.

```jsx
// Infinity
console.log(window.Infinity === Infinity);  // true
console.log(3/0);  // Infinity
console.log(typeof Infinity);  // number

// NaN (Not a Number)
console.log(window.NaN);  // NaN
console.log(typeof NaN);  // number

// undefined
console.log(window.undefined);  // undefined
console.log(typeof undefined);  // undefined
```

### 빌트인 전역 함수

전역 객체의 메서드를 의미한다.

```jsx
// eval
// eval 함수는 자바스크립트 코드를 나타내는 문자열을 인수로 전달받는다.
eval('1 + 2;');  // 3
eval('var x = 5;');  // undefined
console.log(x);  // 5 -> eval 함수에 의해 런타임에 변수 선언문이 실행됨.

// isFinite
// 전달받은 인수가 유한수이면 true를 반환하고, 무한수이면 false를 반환한다.
// 인수의 타입이 숫자가 아닌 경우, 숫자로 타입을 변환후 검사를 수행한다.
isFinite(0);  // true
isFinite(2e64);  // true
isFinite('10');  // true
isFinite(Infinity);  // false
isFinite(NaN);  // false

// isNaN
// 전달받은 인수가 NaN인지 검사하여 결과를 불리언 타입으로 반환한다.
isNaN(NaN); // true
isNaN(10);  // false
isNaN('sung');  // true

// parseFloat
// 전달받은 문자열 인수를 부동 소수점 숫자, 즉 실수로 해석하여 반환한다.
parseFloat('3.14');  // 3.14
parseFloat('10.00'); // 10
parseFloat('He was 40');  // NaN

// parseInt
// 전달받은 문자열 인수를 정수로 해석하여 반환한다.
parseInt('10');  // 10
parseInt('10.234');  // 10
parseInt('10', 2);  // 2
parseInt('10', 8);  // 8

// encodeURI / decodeURI
// encodeURI 함수는 완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.
// decodeURI 함수는 인코딩된 URI를 인수로 저달받아 이스케이프 처리 이전으로 디코딩한다.
const uri = 'http://www.google.com/...';
const enc = encodeURI(uri);
const dec = decodeURI(enc);

// encodeURIComponent / decodeURIComponent
// encodeURIComponent 함수는 URI 구성요소를 인수로 전달받아 인코딩한다.
// decodeURIComponent 함수는 인코딩된 URI 구성요소를 전달받아 이스케이프 처리 이전으로 디코딩한다.
const uriComp = 'name=성&job=student';
let enc = encodeURIComponent(uriComp);
let dec = decodeURIComponent(enc);
```

### 암묵적 전역

```jsx
var x = 10;  // 전역 변수
function foo() {
	// 선언하지 않은 식별자에 값을 할당
	y = 20;  // window.y = 20;
}
foo();

console.log(x+y);  // 30
```

# this

## this 키워드

메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 **************************************************************************************************************************************************자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.**************************************************************************************************************************************************

**********this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.**********

****************************************************************************************************************************************************this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.****************************************************************************************************************************************************

```jsx
const circle = {
	radius: 5,
	getDiameter() {
		return 2 * this.radius;
	}
};

console.log(circle.getDiameter());  // 10

// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this);  // window

// 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
function square(number) {
	console.log(this);  // window
	return number * number;
}
square(2);

// 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
const person = {
	name: 'Lee',
	getName() {
		console.log(this);  // {name: "Lee", getName: f}
		return this.name;
	}
};
console.log(person.getName());  // Lee

// 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
function Person(name) {
	this.name = name;
	console.log(this);  // Person {name: "Lee"}
}

const me = new Person('Lee');
```

## 함수 호출 방식과 this 바인딩

********************************this 바인딩(this에 바인딩될 값)은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.********************************

함수를 호출하는 방식:

```jsx
const foo = function() {
	console.dir(this);
};

// 1. 일반 함수 호출
foo();  // window

// 2. 메서드 호출
const obj = { foo };
obj.foo();  // obj

// 3. 생성자 함수 호출
new foo();  // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
const bar = { name: 'bar' };

foo.call(bar);  // bar
foo.apply(bar); // bar
foo.bind(bar)(); // bar

```

### 일반 함수 호출

****************************************기본적으로 this에는 전역 객체가 바인딩된다.****************************************

전역 함수는 물론이고 중첩 함수를 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩된다.

일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 this에는 전역 객체가 바인딩된다.

### 메서드 호출

```jsx
const person = {
	name: 'Lee',
	getName() {
		// 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
		return this.name;
	}
};

console.log(person.getName());  // Lee
```

### 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩 된다.

```jsx
// 생성자 함수
function Circle(radius) {
	// 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter());  // 10
console.log(circle2.getDiameter());  // 20
```

| 함수 호출 방식 | this 바인딩 |
| --- | --- |
| 일반 함수 호출 | 전역 객체 |
| 메서드 호출 | 메서드를 호출한 객체 |
| 생성자 함수 호출 | 생성자 함수가 (미래에) 생성할 인스턴스 |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | Function.prototype.apply/call/bind 메서드에 첫 번째 인수로 전달한 객체 |

# 실행 컨텍스트 ( 나중에 다시 )

실행 컨텍스트는 자바스크립트의 동작 원리를 담고 있는 핵심 개념이다.

## 소스코드의 타입

| 소스코드의 타입 | 설명 |
| --- | --- |
| 전역 코드 | 전역에 존재하는 소스코드를 말한다. 전역에 정의된 함수, 클래스 등의 내부 코드는 포함하지 않는다. |
| 함수 코드 | 함수 내부에 존재하는 소스코드를 말한다. 함수 내부에 중첩된 함수, 클래스 등의 내부 코드는 포함되지 않는다. |
| eval 코드 | 빌트인 전역 함수인 eval 함수에 인수로 전달되어 실행되는 소스코드를 말한다. |
| 모듈 코드 | 모듈 내부에 존재하는 소스코드를 말한다. 모듈 내부의 함수, 클래스 등의 내부 코드는 포함되지 않는다. |

## 소스코드의 평가와 실행

자바스크립트 엔진은 소스코드를 2개의 과정, 즉 “소스코드의 평가”와 “소스코드의 실행” 과정으로 나누어 처리한다.

소스코드 평가 과정에서는 실행 컨텍스트를 생성하고 변수, 함수 등의 선언문만 먼저 실행하여 생성된 변수나 함수 식별자를 키로 실행 컨텍스트가 관리하는 스코프에 등록한다.

소스코드 평가 과정이 끝나면 비로소 선언문을 제외한 소스코드가 순차적으로 실행되기 시작한다.

## 실행 컨텍스트의 역할

1. 전역 코드 평가
2. 전역 코드 실행
3. 함수 코드 평가
4. 함수 코드 실행

실행 컨텍스트는 소스코드를 실행하는 데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.

실행 컨텍스트는 식별자(변수, 함수, 클래스 등의 이름)를 등록하고 관리하는 스코프와 코드 실행 순서 관리를 구현한 내부 메커니즘으로, 모든 코드는 실행 컨텍스트를 통해 실행되고 관리된다.

식별자와 스코프는 실행 컨텍스트의 **렉시컬 환경**으로 관리하고 코드 실행 순서는 **실행 컨텍스트 스택**으로 관리한다.

## 실행 컨텍스트 스택

```jsx
const x = 1;

function foo () {
	const y = 2;
	
	function bar () {
		const z = 3;
		console.log(x + y + z);
	}
	bar();
}

foo();  // 6
```

1. 전역 코드의 평가와 실행
2. foo 함수 코드의 평가와 실행
3. bar 함수 코드의 평가와 실행
4. foo 함수 코드로 복귀
5. 전역 코드로 복귀

**실행 컨텍스트 스택은 코드의 실행 순서를 관리한다.**

실행 컨텍스트 스택의 최상위에 존재하는 실행 컨텍스트는 언제나 현재 실행 중인 코드의 실행 컨텍스트다.

## 렉시컬 환경

렉시컬 환경은 식별자와 식별자에 바인딩된 값, 그리고 상위 스코프에 대한 참조를 기록하는 자료구조로 실행 컨텍스트를 구성하는 컴포넌트다.

실행 컨텍스트 스택이 코드의 실행 순서를 관리한다면 렉시컬 환경은 스코프와 식별자를 관리한다.

1. 환경 레코드 Environment Record
    - 스코프에 포함된 식별자를 등록하고 등록된 식별자에 바인딩된 값을 관리하는 저장소다.
2. 외부 렉시컬 환경에 대한 참조 Outer Lexical Environment Reference
    - 외부 렉시컬 환경에 대한 참조는 상위 스코프를 가리킨다.

## 실행 컨텍스트의 생성과 식별자 검색 과정

### 전역 객체 생성

전역 객체는 전역 코드가 평가되기 이전에 생성된다.

이때 전역 객체에는 빌트인 전역 프로퍼티와 빌트인 전역 함수, 그리고 표준 빌트인 객체가 추가되며 동작 환경에 따라 클라이언트 사이드 Web Api또는 특정 환경을 위한 호스트 객체를 포함한다.

### 전역 코드 평가

1. 전역 실행 컨텍스트 생성
- 먼저 비어있는 전역 실행 컨텍스트를 생성하여 실행 컨텍스트 스택에 푸쉬한다.
2. 전역 렉시컬 환경 생성
- 전역 렉시컬 환경을 생성하고 전역 실행 컨텍스트에 바인딩한다.
    
    2.1. 전역 환경 레코드 생성
    
    - var 키워드로 선언한 전역 변수와 let, const 키워드로 선언한 전역 변수를 구분하여 관리하기 위해 객체 환경 레코드와 선언적 환경 레코드로 구성되어 있다.
        
        2.1.1. 객체 환경 레코드 생성
        
        2.1.2. 선언적 환경 레코드 생성
        
    
    2.2. this 바인딩
    
    2.3. 외부 렉시컬 환경에 대한 참조 결정
    

### 전역 코드 실행

이제 전역 코드가 순차적으로 실행되기 시작한다.

식별자 결정을 위해 식별자를 검색할 때는 실행 중인 실행 컨텍스트에서 식별자를 검색하기 시작한다.

### foo 함수 코드 평가

1. 함수 실행 컨텍스트 생성
2. 함수 렉시컬 환경 생성
    
    2.1. 함수 렉시컬 환경 생성
    
    2.2. this 바인딩
    
    2.3. 외부 렉시컬 환경에 대한 참조 결정
    

### foo 함수 코드 실행

foo 함수의 소스코드가 순차적으로 실행되기 시작한다.

### bar 함수 코드 평가

### bar 함수 코드 실행

### bar 함수 코드 실행 종료

### foo 함수 코드 실행 종료

### 전역 코드 실행 종료

## 실행 컨텍스트와 블록 레벨 스코프

var 키워드로 선언한 변수는 오로지 함수의 코드 블록만 지역 스코프로 인정하는 함수 레벨 스코프를 따른다.

하지만 let, const 키워드로 선언한 변수는 모든 코드 블록을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.
