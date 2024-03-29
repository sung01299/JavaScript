# JS Day10

# Number

표준 빌트인 객체인 Number는 원시 타입인 숫자를 다룰 때 유용한 프로퍼티와 메서드를 제공 한다.

## Number 생성자 함수

new 연산자와 함께 호출하여 Number 인스턴스를 생성할 수 있다.

```jsx
const numObj = new Number();
console.log(number);  // Number {[[PrimitiveValue]]: 0}
const numObj = new Number(10);
console.log(number);  // Number {[[PrimitiveValue]]: 10}
```

## Number 프로퍼티

### Number.EPSILON

ES6에서 도입된 Number.EPSILON은 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다.

Number.EPSILON은 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다.

```jsx
function isEqual(a, b){
	return Math.abs(a - b) < Number.EPSILON;
}

isEqual(0.1 + 0.2, 0.3);  // true
```

### Number.MAX_VALUE

자바스크립트에서 표현할 수 있는 가장 큰 양수 값이다.

```jsx
Number.MAX_VALUE;  // 1.7976931348623157e+308
Infinity > Number.MAX_VALUE;  // true
```

### Number.MIN_VALUE

자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다.

```jsx
Number.MIN_VALUE;  // 5e-324
Number.MIN_VALUE > 0;  // true
```

### Number.MAX_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수 값이다.

```jsx
Number.MAX_SAFE_INTEGER;  // 9007199254740991
```

### Number.MIN_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수 값이다.

```jsx
Number.MIN_SAFE_INTEGER;  // -9007199254740991
```

### Number.POSITIVE_INFINITY

```jsx
Number.POSITIVE_INFINITY;  // Infinity
```

### Number.NEGATIVE_INFINITY

```jsx
Number.NEGATIVE_INFINITY;  // Infinity
```

### Number.NaN

```jsx
Number.Nan;  // NaN
```

## Number 메서드

### Number.isFinite

Number.isFinite 정적 메서드는 인수로 전달된 숫자값이 정상적인 유한수, 즉 Infinity 또는 -Infinity가 아닌지 검사하여 그 결과를 불리언 값으로 반환한다.

```jsx
Number.isFinite(0);  // true
Number.isFinite(Number.MAX_VALUE);  // true
Number.isFinite(INFINITY);  // false
Number.isFinite(NaN);  // false
```

### Number.isInteger

Number.isInteger 정적 메서드는 인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.

```jsx
Number.isInteger(0);  // true
Number.isInteger(0.5);  // false
Number.isInteger('123');  // false
Number.isInteger(Infinity);  // false
```

### Number.isNaN

Number.isNaN 정적 메서드는 인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환한다.

```jsx
Number.isNaN(NaN);  // true
Number.isNaN(undefined);  // false
```

### Number.isSafeInteger

Number.isSafeInteger 정적 메서드는 인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.

```jsx
Number.isSafeInteger(0);  // true
Number.isSafeInteger(0.5);  // false
Number.isSafeInteger(Infinity);  // false
```

### Number.prototype.toExponential

toExponential 메서드는 숫자를 지수 표기법으로 변환하여 문자열로 반환한다.

```jsx
(77.1234).toExponential();  // "7.71234e+1"
(77.1234).toExponential(2);  // "7.71e+1"
(77).toExponential();  // 7.7e+1
```

### Number.prototype.toFixed

toFixed 메서드는 숫자를 반올림하여 문자열로 반환한다.

```jsx
(12345.6789).toFixed();  // "12346"
(12345.6789).toFixed(1);  // "12345.7"
(12345.6789).toFixed(3);  // "12345.679"
```

### Number.prototype.toPrecision

toPrecision 메서드는 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열을 반환한다.

```jsx
(12345.6789).toPrecision();  // "12345.6789"
(12345.6789).toPrecision(1);  // "1e+4"
(12345.6789).toPrecision(2);  // "1.2e+4"
(12345.6789).toPrecision(6);  // "12345.7"
```

### Number.prototype.toString

toString 메서드는 숫자를 문자열로 변환하여 반환한다.

```jsx
(10).toString();  // "10"
// 2진수 문자열을 반환한다.
(16).toString(2);  // "10000"
// 8진수 문자열을 반환한다.
(16).toString(8);  // "20"
// 16진수 문자열을 반환한다.
(16).toString(16);  // "10"
```

# Math

표준 빌트인 객체인 Math는 수학적인 상수와 함수를 위한 프로퍼티와 메서드를 제공한다.

Math는 생성자 함수가 아니므로 정적 프로퍼티와 정적 메서드만 제공한다.

## Math 프로퍼티

### Math.PI

원주율 PI 값을 반환한다.

```jsx
Math.PI;  // 3.141592653589793
```

## Math 메서드

### Math.abs

인수로 전달된 숫자의 절대값을 반환한다.

```jsx
Math.abs(-1);  // 1
Math.abs('-1');  // 1
Math.abs('');  // 0
Math.abs([]);  // 0
Math.abs(null);  // NaN
```

### Math.round

인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환한다.

```jsx
Math.round(1.4);  // 1
Math.round(1.6);  // 2
Math.round(-1.4);  // -1
Math.round(-1.6);  // -2
Math.round(1);  // 1
```

### Math.ceil

인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환한다.

```jsx
Math.ceil(1.4);  // 2
Math.ceil(1.6);  // 2
Math.ceil(-1.4);  // -1
Math.ceil(-1.6);  // -1
Math.ceil();  // NaN
```

### Math.floor

인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환한다.

```jsx
Math.floor(1.9);  // 1
Math.floor(9.1);  // 9
Math.floor(-1.9);  // -2
Math.floor(-9.1);  // -10
Math.floor();  // NaN
```

### Math.sqrt

인수로 전달된 숫자의 제곱근을 반환한다.

```jsx
Math.sqrt(9);  // 3
Math.sqrt(1);  // 1
Math.sqrt(0);  // 0
```

### Math.random

임의의 난수를 반환한다. 난수는 0에서 1미만의 실수다.

```jsx
Math.random();
```

### Math.pow

첫 번째 인수를 밑으로, 두 번째 인수를 지수로 거듭제곱한 결과를 반환한다.

```jsx
Math.pow(2, 8);  // 256
Math.pow(2, -1);  // 0.5
Math.pow(2);  // NaN
```

### Math.max

전달받은 인수 중에서 가장 큰 수를 반환한다.

```jsx
Math.max( ... [1, 2, 3]);  // 3
```

### Math.min

전달받은 인수 중에서 가장 작은 수를 반환한다.

```jsx
Math.min( ... [1, 2, 3]);  // 1
```

# Date

표준 빌트인 객체인 Date는 날짜와 시간을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수다.

## Date 생성자 함수

Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다.

### new Date()

Date 생성자 함수를 new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다.

```jsx
new Date();  // Mon Jul 06 2020 01:03:18 GMT+0900
Date();  // // Mon Jul 06 2020 01:03:18 GMT+0900
```

### new Date(milliseconds)

Date 생성자 함수에 숫자 타입의 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.

```jsx
new Date(0);  // Thu Jan 01 1970 09:00:00 GMT+0900
new Date(86400000);  // Fri Jan 02 1970 09:00:00 GMT+0900
```

### new Date(dateString)

Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.

이때 인수로 전달한 문자열은 Date.parse 메서드에 의해 해석 가능한 형식이어야 한다.

```jsx
new Date('May 26, 2020 10:00:00');
// Tue May 26 2020 10:00:00 GMT+0900
new Date('2020/03/26/10:00:00');
// Thu Mar 26 2020 10:00:00 GMT+0900
```

### new Date(year, month[, day, hour, minute, second, millisecond])

Date 생성자 함수에 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.

이때 연, 월은 반드시 지정해야 한다.

```jsx
new Date(2020, 2);  // Sun Mar 01 2020 00:00:00 GMT+0900
new Date(2020, 2, 26, 10, 00, 00, 0);  // Thu Mar 26 2020 10:00:00 GMT+0900
new Date('2020/3/26/10:00:00');  // Thu Mar 26 2020 10:00:00 GMT+0900
```

## Date 메서드

### Date.now

1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간가지 경과한 밀리초를 숫자로 반환한다.

```jsx
const now = Date.now();  // 1593971539112
new Date(now);  // Mon Jul 06 2020 02:52:19 GMT+0900
```

### Date.parse

1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.

```jsx
Date.parse('Jan 2, 1970 00:00:00 UTC');  // 86400000
Date.parse('Jan 2, 1970 09:00:00');  // 86400000
```

### Date.UTC

1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.

```jsx
Date.UTC(1970, 0, 2);  // 86400000
Date.UTC('1970/1/2');  // NaN
```

### Date.prototype.getFullYear

```jsx
new Date('2020/07/24').getFullYear();  // 2020
```

### Date.prototype.setFullYear

```jsx
const today = new Date();

today.setFullYear(2000);
today.getFullYear();  // 2000

today.setFullYear(1900, 0, 1);
today.getFullYear();  // 1900
```

### Date.prototype.getMonth

```jsx
new Date('2020/07/24').getMonth();  // 6
```

### Date.prototype.setMonth

```jsx
const today = new Date();

today.setMonth(0);  // Jan
today.getMonth();  // 0

today.setMonth(11, 1);  // Dec 1st
today.getMonth();  // 11
```

### Date.prototype.getDate

```jsx
new Date('2020/07/24').getDate();  // 24
```

### Date.prototype.setDate

```jsx
const today = new Date();

today.setDate(1);
today.getDate();  // 1
```

### Date.prototype.getDay

```jsx
new Date('2020/07/24').getDay();  // 5
```

### Date.prototype.getHours

```jsx
new Date('2020/07/24/12:00').getHours();  // 12
```

### Date.prototype.setHours

```jsx
const today = new Date();

today.setHours(7);
today.getHours();  // 7

today.setHours(0, 0, 0, 0);  // 00:00:00:00
today.getHours();  // 0
```

### Date.prototype.getMinutes

```jsx
new Date('2020/07/24/12:30').getMinutes();  // 30
```

### Date.prototype.setMinutes

```jsx
const today = new Date();

today.setMinutes(50);
today.getMinuts();  // 50

today.setMinutes(5, 10, 999);  // HH:05:10:999
today.getMinutes();  // 5
```

### Date.prototype.getSeconds

```jsx
new Date('2020/07/24/12:30:10').getSeconds();  // 10
```

### Date.prototype.setSeconds

```jsx
const today = new Date();

today.setHours(30);
today.getHours();  // 30

today.setHours(10, 0);  // HH:MM:10:000
today.getHours();  // 10
```

### Date.prototype.getMilliseconds

```jsx
new Date('2020/07/24/12:30:10:150').getMilliseconds();  // 150
```

### Date.prototype.setMilliseconds

```jsx
const today = new Date();

today.setMilliseconds(123);
today.getMilliseconds();  // 123
```

### Date.prototype.getTime

```jsx
new Date('2020/07/24/12:30').getTime();  // 1595561400000
```

### Date.prototype.setTime

```jsx
const today = new Date();

today.setTime(86400000);
console.log(today);  // Fri Jan 02 1970 09:00:00 GMT+0900
```

### Date.prototype.getTimezoneOffset

UTC와 Date 객체에 지정된 로캘 시간과의 차이를 분 단위로 반환한다.

```jsx
const today = new Date();

today.getTimezoneOffset() / 60;  // -9
```

### Date.prototype.toDateString

사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환한다.

```jsx
const today = new Date('2020/7/24/12:30');

today.toString();  // Fri Jun 24 2020 12:30:00 GMT+0900
today.toDateString();  // Fri Jul 24 2020
```

### Date.prototype.toTimeString

```jsx
const today = new Date('2020/7/24/12:30');

today.toString();  // Fri Jul 24 2020 12:30:00 GMT+0900
today.toTimeString();  // 12:30:00 GMT+0900
```

### Date.prototype.toISOString

```jsx
const today = new Date('2020/7/24/12:30');

today.toString();  // Fri Jul 24 2020 12:30:00 GMT+0900
today.toISOString();  // 2020-07-24T03:30:00.000Z
```

### Date.prototype.toLocaleString

```jsx
const today = new Date('2020/7/24/12:30');

today.toString();  // Fri Jul 24 2020 12:30:00 GMT+0900
today.toLocaleString();  // 2020. 7. 24. 오후 12:30:00
```
