# JS Day12

# 이터러블

## 이터레이션 프로토콜

이터레이션 프로토콜은 순회 가능한 데이터 컬렉션을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙이다.

ES6에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 for … of 문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화 했다.

### 이터러블

이터러블 프로토콜을 준수한 객체를 이터러블이라 한다.

즉, 이터러블은 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 말한다.

```jsx
// 이터러블인지 확인하는 함수는 다음과 같이 구현할 수 있다.
const isIterable = v => v !== null && typeof v[Symbol.iterator] === 'function';

isIterable([]);  // true
isIterable('');  // true
isIterable(new Map());  // true
isIterable({});  // false

// 이터러블은 for ... of 문으로 순회할 수 있으며, 스프레드 무법과 배열 디스트럭처링 할당의 대상
// 으로 사용할 수 있다.
const array = [1, 2, 3];

console.log(Symbol.iterator in array);  // true
for (const item of array) {
	console.log(item);
}

console.log([...array]);  // [1, 2, 3]

const [a, ...rest] = array;
console.log(a, rest);  // 1, [2, 3]
```

### 이터레이터

이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다.

**이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.**

```jsx
const array = [1, 2, 3];
const iterator = array[Symbol.iterator]();
console.log('next' in iterator);  // true

// 이터레이터의 next 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할을 한다.
// 즉, next 메서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과를 나타내는
// 이터레이터 리절트 객체를 반환한다.

const array = [1, 2, 3];
const iterator = array[Symbol.iterator]();

console.log(iterator.next());  // {value: 1, done: false}
console.log(iterator.next());  // {value: 2, done: false}
console.log(iterator.next());  // {value: 3, done: fales}
console.log(iterator.next());  // {value: undefined, done: true}
```

## 빌트인 이터러블

자바스크립트는 이터레이션 프로토콜을 준수한 객체인 빌트인 이터러블을 제공한다.

다음의 표준 빌트인 객체들은 빌트인 이터러블이다.

| 빌트인 이터러블 | Symbol.iterator 메서드 |
| --- | --- |
| Array | Array.prototype[Symbol.iterator] |
| String | String.prototype[Symbol.iterator] |
| Map | Map.prototype[Symbol.iterator] |
| Set | Set.prototype[Symbol.iterator] |
| TypedArray | TypedArray.prototype[Symbol.iterator] |
| arguments | arguments.prototype[Symbol.iterator] |
| DOM 컬렉션 | NodeList.prototype[Symbol.iterator]
HTMLCollection.prototype[Symbol.iterator] |

## for … of 문

for … in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거한다. 이때 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.

for … of 문은 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값을 for … of 문의 변수에 할당한다. 그리고 이터레이터 리절트 객체의 done 프로퍼티 값이 false이면 이터러블의 순회를 계속하고 true이면 이터러블의 순회를 중단한다.

```jsx
for (const item of [1, 2, 3]) {
	console.log(item);  // 1 2 3
}
```

## 이터러블과 유사 배열 객체

유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말한다.

유사 배열 객체는 length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있고, 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로 가지므로 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.

```jsx
const arrayLike = {
	0: 1,
	1: 2,
	2: 3,
	length: 3
};

for (let i = 0; i < arrayLike.length; i++) {
	console.log(arrayLike[i]);  // 1 2 3
}

// 유사 배열 객체는 이터러블이 아닌 일반 객체이므로 for ... of 문으로 순회할 수 없다.
for (const item of arrayLike) {
	console.log(item);  // TypeError: arrayLike is not iterable
}

// Array.from 메서드를 사용하여 배열로 간단히 변환할 수 있다.
const arr = Array.from(arrayLike);
console.log(arr);  // [1, 2, 3]
```

## 사용자 정의 이터러블

### 사용자 정의 이터러블 구현

이터레이션 프로토콜을 준수하지 않는 일반 객체도 이터레이션 프로토콜을 준수하도록 구현하면 사용자 정의 이터러블이 된다.

사용자 정의 이터러블은 이터레이션 프로토콜을 준수하도록 Symbol.iterator 메서드를 구현하고 Symbol.iterator 메서드가 next 메서드를 갖는 이터레이터를 반환하도록 한다. 그리고 이터레이터의 next 메서드는 done과 value 프로퍼티를 가지는 이터레이터 리절트 객체를 반환한다.

```jsx
const fibonacci = {
	[Symbol.iterator]() {
		let [pre, cur] = [0, 1];
		const max = 10;
		
		return {
			next() {
				[pre, cur] = [cur, pre + cur];
				return { value: cur, done: cur >= max };
			}
		};
	}
};

for (const num of fibonacci) {
	console.log(num);  // 1 2 3 5 8
}
```

### 이터러블을 생성하는 함수

```jsx
const fibonacciFunc = function (max) {
	let [pre, cur] = [0, 1];

	return {
		[Symbol.iterator]() {
			return {
				next() {
					[pre, cur] = [cur, pre + cur];
					return { value: cur, done: cur >= max };
				}
			};
		}
	};
};

for (const num of fibonacciFunc(10)) {
	console.log(num);  // 1 2 3 5 8
}
```

### 이터러블이면서 이터레이터인 객체를 생성하는 함수

```jsx
const iterable = fibonacciFunc(5);
const iterator = iterable[Symbol.iterator]();

console.log(iterator.next());  // {value: 1, done: false}
console.log(iterator.next());  // {value: 2, done: false}
console.log(iterator.next());  // {value: 3, done: false}
console.log(iterator.next());  // {value: 5, done: true}

// 이터러블이면서 이터레이터인 객체를 생성하면 Symbol.iterator 메서드를 호출하지 않아도 된다.
{
	[Symbol.iterator]() { return this; },
	next() {
		return { value: any, done: boolean };
	}
}
```

### 무한 이터러블과 지연 평가

```jsx
const fibonacciFunc = function () {
	let [pre, cur] = [0, 1];

	return {
		[Symbol.iterator]() { return this; },
		next() {
			[pre, cur] = [cur, pre + cur];
			return { value: cur };
		}
	};
};

for (const num of fibonacciFunc()) {
	if (num > 10000) break;
	console.log(num);  // 1 2 3 5 8 ... 4181 6765
}

const [f1, f2, f3] = fibonacciFunc();
console.log(f1, f2, f3);  // 1 2 3
```

# 스프레드 문법

스프레드 문법 … 은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다.

스프레드 문법을 사용할 수 있는 대상은 Array, String, Map, Set, DOM 컬렉션, arguments와 같이 for … of 문으로 순회할 수 있는 이터러블에 한정된다.

스프레드 문법의 결과는 변수에 할당할 수 없다.

```jsx
console.log(...[1, 2, 3]);  // 1 2 3
console.log(...'Hello');  // H e l l o
console.log(... new Map([['a', '1'], ['b', '2']]));  // ['a', '1']['b', '2']
console.log(... new Set([1, 2, 3]));  // 1 2 3
console.log(...{ a: 1, b: 2 });
// TypeError: Found non-callable @@iterator
```

## 함수 호출문의 인수 목록에서 사용하는 경우

```jsx
const arr = [1, 2, 3];
const max = Math.max(arr);  // NaN
const max = Math.max(...arr);  // 3
```

Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받기 위해 매개변수 이름 앞에 … 을 붙이는 것이다.

스프레드 문법은 여러 개의 값이 하나로 뭉쳐 있는 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만드는 것이다.

따라서 Rest 파라미터와 스프레드 문법은 서로 반대의 개념이다.

```jsx
function foo(...rest){
	console.log(rest);  // 1, 2, 3 -> [1, 2, 3]
}

foo(...[1, 2, 3]);  // [1, 2, 3] -> 1, 2, 3
```

## 배열 리터럴 내부에서 사용하는 경우

스프레드 문법을 배열 리터럴에서 사용하면 ES5에서 사용하던 기존의 방식보다 더욱 간결하고 가독성 좋게 표현할 수 있다.

### concat

```jsx
var arr = [1, 2].concat([3, 4]);
console.log(arr);  // [1, 2, 3, 4]

const arr = [...[1, 2], ...[3, 4]];
console.log(arr);  // [1, 2, 3, 4]
```

### splice

```jsx
var arr1 = [1, 4];
var arr2 = [2, 3];
Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
console.log(arr1);  // [1, 2, 3, 4]

const arr1 = [1, 4];
const arr2 = [2, 3];
arr1.splice(1, 0, ...arr2);
console.log(arr1);  // [1, 2, 3, 4]
```

### 배열 복사

```jsx
var origin = [1, 2];
var copy = origin.slice();
console.log(copy);  // [1, 2]
console.log(copy === origin);  // false

const origin = [1, 2];
const copy = [...origin];
console.log(copy);  // [1, 2]
console.log(copy === origin);  // false
```

### 이터러블을 배열로 반환

```jsx
function sum() {
	var args = Array.prototype.slice.call(arguments);
	return args.reduce(function (pre, cur) {
		return pre + cur;
	}, 0);
}

console.log(sum(1, 2, 3));  // 6

function sum() {
	return [...arguments].reduce((pre, cur) => pre + cur, 0);
}
console.log(sum(1, 2, 3));  // 6
```

# 디스트럭처링 할당

디스트럭처링 할당 (구조 분해 할당)은 구조화된 배열과 같은 이터러블 또는 객체를 destructuring 하여 1개 이상의 변수에 개별적으로 할당하는 것을 말한다.

배열과 같은 이터러블 또는 객체 리터럴에서 필요한 값만 추출하여 변수에 할당할 때 유용하다.

## 배열 디스트럭처링 할당

배열 디스트럭처링 할당의 대상(할당문의 우변)은 이터러블이어야 하며, 할당 기준은 배열의 인덱스다.

```jsx
const arr = [1, 2, 3];
const [one, two, three] = arr;
console.log(one, two, three);  // 1 2 3

const [a, b] = [1, 2];
console.log(a, b);  // 1 2
const [c, d] = [1];
console.log(c, d);  // 1 undefined
const [e, f] = [1, 2, 3];
console.log(e, f);  // 1 2
const [g, , h] = [1, 2, 3];
console.log(g, h);  // 1 3
const [a, b, c = 3] = [1, 2];
console.log(a, b, c);  // 1 2 3

// 배열 디스트럭처링 할당을 위한 변수에 Rest 파라미터와 유사하게 Rest 요소 ... 을 사용할 수 있다.
const [x, ... y] = [1, 2, 3];
console.log(x, y);  // 1 [2, 3]
```

## 객체 디스트럭처링 할당

객체 디스트럭처링 할당은 객체의 각 프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당한다.

이때 객체 디스트럭처링 할당의 대상(할당문의 우변)은 객체이어야 하며, 할당 기준은 프로퍼티 키다.

```jsx
const user = { firstName: 'Sung', lastName: 'Huh' };
const { lastName, firstName } = user;
console.log(firstName, lastName);  // Sung Huh

const { lastName: ln, firstName: fn } = user;
console.log(fn, ln);  // Sung Huh

// 객체 디스트럭처링 할당은 객체에서 프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당하고
// 싶을 때 유용하다.
const str = 'Hello';
const { length } = str;
console.log(length);  // 5

const todo = { id: 1, content: 'HTML', completed: true };
const { id } = todo;
console.log(id);  // 1

// 배열의 요소가 객체인 경우 배열 디스트럭처링 할당과 객체 디스트럭처링 할당을 혼용할 수 있다.
const [, { id }] = todos;
console.log(id);  // 2

// 중첩 객체의 경우
const { address: { city } } = user;
console.log(city);  // Seoul
```
