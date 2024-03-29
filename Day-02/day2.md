# 데이터 타입
## 데이터 타입의 종류
- 원시 타입 (Primitive types)
  - 숫자 (Number)
  - 문자열 (String)
  - 불리언 (Boolean)
  - Undefined
  - null
  - symbol
- 객체 타입 (Object)

## 데이터 타입이 필요한 이유
1. 값을 저장할 때 확보해야 하는 **메모리 공간의 크기**를 결정하기 위해 
2. 값을 참조할 때 한 번에 읽어 들여야 할 **메모리 공간의 크기**를 결정하기 위해
3. 메모리에서 읽어 들인 **2진수를 어떻게 해석**할지 결정하기 위해

## 동적 타이핑 (Dynamic Typing)
- 자바스크립트의 변수는 선언이 아닌 할당에 의해 타입이 결정 된다.
- 그리고 재할당에 의해 변수의 타입은 언제든지 동적으로 변할 수 있다.

```javascript
var foo;
console.log(typeof foo);    // undefined

foo = 3;
console.log(typeof foo);    // number

foo = 'Hello';
console.log(typeof foo);    // string

foo = true;
console.log(typeof foo);    // boolean

foo = null;
console.log(typeof foo);    // object  -> Bug

foo = Symbol();
console.log(typeof foo);    // symbol

foo = {};
console.log(typeof foo);    // object

foo = [];
console.log(typeof foo);    // object

foo = function () {};
console.log(typeof foo);    // function
```

# 연산자
## 연산자의 종류
### 산술 연산자 (Arithmetic operator)
  - 이항 산술 연산자 (Binary)
    - +, -, *, /, %, **(Math.pow)
  - 단항 산술 연산자 (Unary)
    - ++, --, +, -
    - ```javascript
      var x = 5, result;
      // postfix increment operator
      result = x++;
      console.log(result, x);   // 5 6
      
      // prefix increment operator
      result = ++x;
      console.log(result, x);   // 7 7

      // postfix decrement operator
      result = x--;
      console.log(result, x);   // 7 6
      
      // prefix decrement operator
      result = --x;
      console.log(result, x);   // 5 5
      ```
### 문자열 연결 연산자
- ```Javascript
    '1' + 2;      // '12'
    1 + true;     // 2
    1 + false;    // 1
    1 + null;     // 1
    1 + undefined; // NaN
  ```

### 할당 연산자 (Assignment Operator)
  - =, +=, -=, *=, /=, %=, **=

### 비교 연산자 (Comparison operator)
  - 동등/일치 비교 연산자
    - == (compare value)
    - === (compare value and type)
  - 대소 관계 비교 연산자
    - ">, <, >=, <="

### 삼항 조건 연산자 (Ternary Operator)
  - 조건식 ? 조건식이 true일 때 반환할 값 : 조건식이 false일 때 반환할 값

### 논리 연산자 (Logical Operator)
  - ||, &&, !

### typeof 연산자
  - "string", "number", "boolean", "undefined", "symbol", "object", "function"

## 연산자 우선순위
  - ()
  - new, ., [], (), ?.
  - new
  - x++, x--
  - !x, +x, -x, ++x, --x, typeof, delete
  - **
  - *, /, %
  - +, -
  - <, <=, >, >=, in, instanceof
  - ==, !=, ===, !==
  - ??
  - &&
  - ||
  - ? ... : ...
  - =, +=, -=, ...
  - ,

# 제어문
## 블록문 (Block/compound Statement)
- 0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고 부른다.
- 블록문은 언제나 문의 종료를 의미하는 자체 종결성을 갖기 때문에 블록문의 끝에는 세미콜론을 붙이지 않는다.
  
## 조건문 (Conditional Statement)
### If ... else 문
  - ```javascript
    if (statement) {

    } else if (statement 2) {

    } else {

    }

    var x = 2;
    var result;

    if (x % 2) {
        result = 'odd';
    } else {
        result = 'even';
    }

    var result = x % 2 ? 'odd' : 'else';
    
    var kind = num ? ( num > 0 ? 'positive' : 'negative' ) : 'zero';
    ```
### switch 문
- switch 문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case 문으로 실행 흐름을 옮긴다.
- case 문은 상황을 의미하는 표현식을 지정하고 콜론으로 마친다.
- switch 문의 표현식과 일치하는 case 문이 없다면 실행순서는 default 문으로 이동한다.
- ```javascript
    switch (statement) {
        case statement1:
            switch ~~
            break;
        case statement2:
            switch ~~
            break;
        default:
            switch ~~
    }
  ```

## 반복문 (Loop Statement)
### for 문
```javascript
for (var i = 0; i < 2; i++){
    console.log(i); // 0, 1
}

// infinite loop
for (;;) {
    ... 
}

// 중첩 for 문
for (var i = 1; i <= 6; i++) {
    for (var j = 1; j <= 6; j++) {
        if (i+j === 6) console.log(`[${i}, ${j}]`);
    }
}
```

### while 문
```javascript
var count = 0;
while (count < 3) {
    console.log(count); // 0 1 2
    count++;
}

// infinite loop
while (true) {}

// to break from infinite loop
while (true) {
    console.log(count);
    count++;
    if (count === 3) break;
} // 0 1 2
```

### do ... while 문
- 코드 블록을 먼저 실행하고 조건식을 평가한다.
- 따라서 코드 블록은 무조건 한 번 이상 실행된다.
- ```javascript
    var count = 0;
    do {
        console.log(count);
        count++;
    } while (count < 3);
  ```

### break 문
- 레이블 문, 반복문, 또는 switch 문의 코드 블록을 탈출한다.

### continue 문
- 반복문의 코드 블록 실행을 현 시점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다.
- break 문처럼 반복문을 탈출하지는 않는다.
- ```javascript
    var string = "Hello World";
    var search = "l";
    var count = 0;

    for (var i = 0; i < string.length; i++) {
        if (string[i] !== search) continue;
        count++;
    }

    console.log(count);   // 3

    const regexp = new RegExp(search, 'g');
    console.log(string.match(regexp).length);  // 3
  ```