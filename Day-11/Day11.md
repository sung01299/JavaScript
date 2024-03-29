# JS Day11

# RegExp

## 정규 표현식이란?

정규 표현식은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어다.

정규 표현식은 문자열을 대상으로 **패턴 매칭 기능**을 제공하며, 패턴 매칭 기능이란 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말한다.

정규 표현식을 사용하면 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크할 수 있다. 다만 정규 표현식은 주석이나 공백을 허용하지 않고 여러가지 기호를 혼합하여 사용하기 때문에 가독성이 좋지 않다는 문제가 있다.

## 정규 표현식의 생성

정규 표현식 객체를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있다.

```jsx
const target = 'Is this all there is?';

// 패턴: is
// 플래그: i => 대소문자를 구별하지 않고 검색한다.
const regexp = /is/i;

regexp.test(target);  // true

const target = 'Is this all there is?';

const regexp = new RegExp(/is/i);
// const regexp = new RegExp(/is/, 'i');
// const regexp = new RegExp('is', 'i');

regexp.test(target);  // true
```

## RegExp 메서드

### RegExp.prototype.exec

exec 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다.

```jsx
const taret = 'Is this all there is?';
const regExp = /is/;

regExp.exec(target);
// ["is", index = 5, input: "Is this all there is?", groups: undefined]
```

### RegExp.prototype.test

test 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

```jsx
const target = 'Is this all there is?';
const regExp = /is/;

regExp.test(target);  // true
```

### String.prototype.match

match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.

```jsx
const target = 'Is this all there is?';
const regExp = /is/;

target.match(regExp);
// ["is", index = 5, input: "Is this all there is?", groups: undefined]
```

## 플래그

패턴과 함께 정규 표현식을 구성하는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.

| 플래그 | 의미 | 설명 |
| --- | --- | --- |
| i | Ignore case | 대소문자를 구별하지 않고 패턴을 검색한다. |
| g | Global | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다. |
| m | Multi line | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다. |

## 패턴

정규 표현식은 패턴과 플래그로 구성된다.

정규 표현식의 패턴은 문자열의 일정한 규칙을 표현하기 위해 사용하며, 정규 표현식의 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.

### 문자열 검색

정규 표현식의 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색한다.

```jsx
const target = 'Is this all there is?';
const regExp = /is/;

regExp.test(target);  // true
target.match(regExp);  // ["is", index: 5, input: "Is this all there is?, groups: undefined]
```

### 임의의 문자열 검색

.은 임의의 문자 한 개를 의미한다.

.을 3개 연속하여 패턴을 생성했으므로 문자의 내용과 상관없이 3자리 문자열과 매치한다.

```jsx
const target = 'Is this all there is?';

const regExp = / ... /g;

target.match(regExp);
// ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

### 반복 검색

```jsx
// {m, n}은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미한다.
const target = 'A AA B BB Aa Bb AAA';
const regExp = /A{1, 2}/g;

target.match(regExp);  // ["A", "AA", "A", "AA", "A"]

// {n}은 앞선 패턴이 n번 반복되는 문자열을 의미한다.
// 즉, {n}은 {n, n}과 같다.
const regExp = /A{2}/g;

target.match(regExp);  // ["AA", "AA"]

// {n, }은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.
const regExp = /A{2,}/g;

taret.match(regExp);  // ["AA", "AAA"]

// +는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미한다.
const regExp = /A+/g;

target.match(regExp);  // ["A", "AA", "A", "AAA"]

// ?는 앞선 패턴이 최대 한 번 이상 반복되는 문자열을 의미한다.
const target = 'color colour';
const regExp = /colou?r/g;

target.match(regExp);  // ["color", "colour"]
```

### OR 검색

```jsx
// '|' 는 or의 의미를 갖는다. 다음 예제의 /A|B/는 'A' 또는 'B'를 의미한다.
const target = 'A AA B BB Aa Bb';
const regExp = /A|B/g';

target.match(regExp);  // ["A", "A", "A", "B", "B", "B", "A", "B"]

// 분해되지 않은 단어 레벨로 검색하기 위해서는 +를 함께 사용한다.
// 범위를 지정하려면 []내에 -를 사용한다.
// \d는 숫자를 의미한다. \D는 문자를 의미한다. \w는 알파벳, 숫자, 언더스코어를 의미한다.
// \W는 문자를 의미한다.
```

### NOT 검색

```jsx
// [ ... ] 내의 ^은 not의 의미를 갖는다.
const target = 'AA BB 12 Aa Bb';
const regExp = /[^0-9]+/g;

target.match(regExp);  // ["AA BB ", " Aa Bb"]
```

### 시작 위치로 검색

```jsx
// [ ... ] 밖의 ^은 문자열의 시작을 의미한다.
const target = 'https://google.com';
const regExp = /^https/;

regExp.test(target);  // true
```

### 마지막 위치로 검색

```jsx
// $는 문자열의 마지막을 의미한다.
const target = 'https://google.com';
const regExp = /com$/;

regExp.test(target);  // true
```

## 자주 사용하는 정규표현식

### 특정 단어로 시작하는지 검사

```jsx
// 문자열이 'http://' 또는 'https://'로 시작하는지 검사한다.
const url = 'https://example.com';

/^https?:\/\//.test(url);  // true
```

### 특정 단어로 끝나는지 검사

```jsx
const fileName = 'index.html';

/html$/.test(fileName);  // true
```

### 숫자로만 이루어진 문자열인지 검사

```jsx
const target = '12345';

/^d+$/.test(target);  // true
```

### 하나 이상의 공백으로 시작하는지 검사

```jsx
const target = ' Hi!';

/^[\s]+/.test(target);  // true
```

### 아이디로 사용 가능한지 검사

```jsx
const id = 'abc123';

/^[A-Za-z0-9]{4, 10}$/.test(id);  // true
```

### 메일 주소 형식에 맞는지 검사

```jsx
const email = "sung@gmail.com";

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.
test(email);  // true
```

### 핸드폰 번호 형식에 맞는지 검사

```jsx
const cellphone = '010-1234-5678';

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone);  // true
```

### 특수 문자 포함 여부 검사

```jsx
const target = 'abc#123';

(/[^A-Za-z0-9]\gi).test(target);  // true
```

# String

원시 타입인 문자열을 다룰때 유용한 프로퍼티와 메서드를 제공한다.

## String 생성자 함수

new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있다.

```jsx
const strObj = new String();
console.log(strObj);  // String {length: 0, [[PrimitiveValue]]: ""}

const strObj = new String('Lee');
console.log(strObj);  // String {0: "L", 1: "e", 2: "e", length: 3, [[P.V]]: "Lee"}

console.log(strObj[0]);  // L

strObj[0] = 'S';
console.log(strObj);  // Lee
```

## length 프로퍼티

length 프로퍼티는 문자열의 문자 개수를 반환한다.

```jsx
'Hello'.length;  // 5
'안녕하세요'.length;  // 6
```

## String 메서드

String 객체에는 원본 String 래퍼 객체를 직접 변경하는 메서드는 존재하지 않는다.

즉, String 객체의 메서드는 언제나 새로운 문자열을 반환한다.

문자열은 변경 불가능한 원시 값이기 때문에 String 래퍼 객체도 읽기 전용 객체로 제공된다.

### String.prototype.indexOf

indexOf 메서드는 대상 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환한다.

검색에 실패하면 -1을 반환한다.

```jsx
const str = 'Hello World';

str.indexOf('l');  // 2
str.indexOf('or');  // 7
str.indexOf('x');  // -1

// indexOf 메서드의 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
str.indexOf('l', 3);  // 3
```

### String.prototype.search

search 메서드는 대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다.

```jsx
const str = 'Hello world';

str.search(/o/);  // 4
str.search(/x/);  // -1
```

### String.prototype.includes

includes 메서드는 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 그 결과를 true 또는 false로 반환한다.

```jsx
const str = 'Hello world';

str.includes('Hello');  // true
str.includes('');  // true
str.includes('x');  // false
str.includes();  // false

// includes 메서드의 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
str.includes('l', 3);  // true
str.includes('H', 3);  // false
```

### String.prototype.startsWith

startsWith 메서드는 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 그 결과를 true 또는 false로 반환한다.

```jsx
const str = 'Hello world';

str.startsWith('He');  // true
str.startsWith('x');  // false

// startsWith 메서드의 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
str.startsWith(' ', 5);  // true
```

### String.prototype.endsWith

endsWith 메서드는 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 그 결과를 true 또는 false로 반환한다.

```jsx
const str = 'Hello world';

str.endsWith('ld');  // true
str.endsWith('x');  // false

// endsWith 메서드의 2번째 인수로 검색할 문자열의 길이로 전달할 수 있다.
// 문자열 str의 처음부터 5자리 까지 'lo'로 끝나는지 확인
str.endsWith('lo', 5);  // true
```

### String.prototype.charAt

charAt 메서드는 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환한다.

```jsx
const str = 'Hello';

for (let i = 0; i< str.length; i++){
	console.log(str.charAt(i));  // H e l l o
}

// 인덱스가 문자열의 범위를 벗어난 경우 빈 문자열을 반환한다.
str.charAt(5);  // ''
```

### String.prototype.substring

substring 메서드는 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자 바로 이전 문자 까지의 부분 문자열을 반환한다.

```jsx
const str = 'Hello World';

str.substring(1, 4);  // ell

// 2번째 인수를 생략할 경우 첫 번째 인수로 전달한 인덱스에 위치하는 문자부터 마지막 문자까지
// 부분 문자열을 반환한다.
str.substring(1);  // 'ello World'
```

### String.prototype.slice

slice 메서드는 substring 메서드와 동일하게 동작한다. 단 slice 메서드에는 음수인 인수를 전달할 수 있다.

음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환한다.

```jsx
const str = 'hello world';

str.substring(0,5);  // hello
str.slice(0, 5);  // hello

str.substring(2);  // llo world
str.slice(2);  // llo world

str.substring(-5);  // hello world
str.slice(-5);  // world
```

### String.prototype.toUpperCase

toUpperCase 메서드는 대상 문자열을 모두 대문자로 변경한 문자열을 반환한다.

```jsx
const str = 'Hello World';

str.toUpperCase();  // 'HELLO WORLD'
```

### String.prototype.toLowerCase

toLowerCase 메서드는 대상 문자열을 모두 소문자로 변경한 문자열을 반환한다.

```jsx
const str = 'Hello World';

str.toLowerCase();  // 'hello world'
```

### String.prototype.trim

trim 메서드는 대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.

```jsx
const str = '  foo  ';

str.trim();  // 'foo'
```

### String.prototype.repeat

repeat 메서드는 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환한다.

인수로 전달받은 정수가 0이면 빈 문자열을 반환하고, 음수이면 RangeError를 발생시킨다.

```jsx
const str = 'abc';

str.repeat();  // ''
str.repeat(0);  // ''
str.repeat(1);  // 'abc'
str.repeat(2);  // 'abcabc'
str.repeat(2.5);  // 'abcabc'
str.repeat(-1);  // RangeError: Invalid count vaue
```

### String.prototype.replace

replace 메서드는 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.

```jsx
const str = 'Hello world';

str.replace('world', 'Lee');  // Hello Lee
```

### String.prototype.split

split 메서드는 대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환한다.

인수로 빈 문자열을 전달하면 각 문자를 모두 분리하고, 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.

```jsx
const str = 'How are you doing?';

str.split(' ');  // ["How", "are", "you", "doing?"]
str.split(/\s/);  // ["How", "are", "you", "doing?"]
str.split('');  // ["H", "o", "w", ... "i", "n", "g", "?"]
str.split();  // ["How are you doing?]

// 2 번째 인수로 배열의 길이를 지정할 수 있다.
str.split(' ', 3);  // ["How", "are", "you"]
```

# 7 번째 데이터 타입 Symbol

## 심벌이란?

심벌은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다.

심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다. 따라서 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.

## 심벌 값의 생성

### Symbol 함수

심벌 값은 Symbol 함수를 호출하여 생성한다. 다른 원시값은 리터럴 표기법을 통해 값을 생성할 수 있지만 심벌 값은 Symbol 함수를 호출하여 생성해야 한다.

이때 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 절대 중복되지 않는 유일무이한 값이다.

```jsx
const mySymbol = Symbol();
console.log(typeof mySymbol);  // symbol

new Symbol();  // TypeError: Symbol is not a constructor
```

### Symbol.for / Symbol.keyFor 메서드

Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.

검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환한다.

검색에 실패하면 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.

```jsx
const s1 = Symbol.for('mySymbol');
const s2 = Symbol.for('mySymbol');

console.log(s1 === s2);  // true
```

Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

```jsx
const s1 = Symbol.for('mySymbol');
Symbol.keyFor(s1);  // mySymbol

const s2 = Symbol('foo');
Symbol.keyFor(s2);  // undefined
```

## 심벌과 상수

```jsx
const Direction = {
	UP: 1,
	DOWN: 2,
	LEFT: 3,
	RIGHT: 4
};

const myDirection = Direction.UP;
if (myDirection === Direction.UP) {
	console.log('You are going UP');
}

// 위 예제와 같이 값에는 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우가 있다. 이때 문제는
// 상수 값 1, 2, 3, 4가 변경될 수 있으며, 다를 변수 값과 중복될 수도 있다는 것이다.
// 이러한 경우 변경/중복될 가능성이 있는 무의미한 상수 대신 중복될 가능성이 없는 유일무이한 심벌
// 값을 사용할 수 있다.

const Direction = {
	UP: Symbol('up'),
	DOWN: Symbol('down'),
	LEFT: Symbol('left'),
	RIGHT: Symbol('right')
};

const myDirection = Direction.UP;
if (myDirection === Direction.UP) {
	console.log('You are going UP.');
}
```

## 심벌과 프로퍼티 키

객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며, 동적으로 생성할 수도 있다.

심벌 값을 프로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다.

프로퍼티에 접근할 때도 마찬가지로 대괄호를 사용해야 한다.

**심벌 값은 유일무이한 값으로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.**

```jsx
const obj = {
	[Symbol.for('mySymbol')]: 1
};

obj[Symbol.for('mySymbol')];  // 1
```

## 심벌과 프로퍼티 은닉

심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for … in 문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없다.

이처럼 심벌 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하면 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있다.

```jsx
const obj = {
	[Symbol('mySymbol')]: 1
};

for (const key in obj) {
	console.log(key);  // 아무것도 출력되지 않는다.
}

console.log(Object.keys(obj));  // []
console.log(Object.getOwnPropertyNames(obj));  // []

// 하지만 Object.getOwnPropertySymbols 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여
// 생성한 프로퍼티를 찾을 수 있다.
console.log(Object.getOwnPropertySymbols(obj));  // [Symbol(mySymbol)]
const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[symbolKey1]);  // 1
```

## 심벌과 표준 빌트인 객체 확장

일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다.

표준 빌트인 객체는 읽기 전용으로 사용하는 것이 좋다.

하지만 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 표준 빌트인 객체의 기존 프로퍼티 키와 충돌하지 않는 것은 물론, 표준 사양의 버전이 올라감에 따라 추가될지 모르는 어떤 프로퍼티 키와도 충돌할 위험이 없어 안전하게 표준 빌트인 객체를 확장할 수 있다.

```jsx
Array.prototype[Symbol.for('sum')] = function() {
	return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for('sum')]();  // 3
```

## Well-known Symbol

자바스크립트가 기본 제공하는 빌트인 심벌 값을 **Well-known Symbol**이라 부른다.
