# JS Day13

# Set과 Map

## Set

Set 객체는 중복되지 않는 유일한 값들의 집합이다.

Set 객체의 특성은 수학적 집합의 특성과 일치하고 수학적 집합을 구현하기 위한 자료구조다.

| 구분 | 배열 | Set 객체 |
| --- | --- | --- |
| 동일한 값을 중복하여 포함할 수 있다. | O | X |
| 요소 순서에 의미가 있다. | O | X |
| 인덱스로 요소에 접근할 수 있다. | O | X |

### Set 객체의 생성

```jsx
// Set 객체는 Set 생성자 함수로 생성한다.
const set = new Set();
console.log(set);  // Set(0) {}

// Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다.
// 이때 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.
const set1 = new Set([1, 2, 3, 3]);
console.log(set1);  // Set(3) {1, 2, 3}

const set2 = new Set('hello');
console.log(set2);  // Set(4) {"h", "e", "l", "o"}

// 중복을 허용하지 않는 Set 객체의 특성을 활용하여 배열에서 중복된 요소를 제거할 수 있다.
const uniq = array => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4]));  // [2, 1, 3, 4]
```

### 요소 개수 확인

```jsx
// Set 객체의 요소 개수를 확인할 때는 Set.prototype.size 프로퍼티를 사용한다.
const {size} = new Set([1, 2, 3, 3]);
console.log(size);  // 3
```

### 요소 추가

```jsx
// Set 객체에 요소를 추가할 때는 Set.prototype.add 메서드를 사용한다.
const set = new Set();
console.log(set);  // Set(0) {}

set.add(1);
console.log(set);  // Set(1) {1}

set.add(1).add(2);
console.log(set);  // Set(2) {1, 2}
```

### 요소 존재 여부 확인

```jsx
// Set 객체에 특정 요소가 존재하는지 확인하려면 Set.prototype.has 메서드를 사용한다.
const set = new Set([1, 2, 3]);
console.log(set.has(2));  // true
console.log(set.has(4));  // false
```

### 요소 삭제

```jsx
// Set 객체의 특정 요소를 삭제하려면 Set.prototype.delete 메서드를 사용한다.
// delete 메서드에는 인덱스가 아니라 삭제하려는 요소값을 인수로 전달해야 한다.
const set = new Set([1, 2, 3]);
set.delete(2);
console.log(set);  // Set(2) {1, 3}

set.delete(1);
console.log(set);  // Set(1) {3}

set.delete(0);
console.log(set);  // Set(1) {3}

set.delete(1).delete(2);  // TypeError: set.delete.delete is not a function
```

### 요소 일괄 삭제

```jsx
// Set 객체의 모든 요소를 일괄 삭제하려면 Set.prototype.clear 메서드를 사용한다.
const set = new Set([1, 2, 3]);
set.clear();
console.log(set);  // Set(0) {}
```

### 요소 순회

Set 객체의 요소를 순회하려면 Set.prototype.forEach 메서드를 사용한다.

- 첫 번째 인수: 현재 순회 중인 요소값
- 두 번째 인수: 현재 순회 중인 요소값
- 세 번째 인수: 현재 순회 중인 Set 객체 자체

```jsx
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
*/

// Set 객체는 이터러블이므로 for ... of 문, 스프레드 문법, 배열 디스트럭처링의 대상이 될 수 있다.
for (const value of set) {
	console.log(value);  // 1 2 3
}

console.log([...set]);  // [1, 2, 3]

const [a, ...rest] = set;
console.log(a, rest);  // 1, [2, 3]
```

### 집합 연산

```jsx
// 교집합
Set.prototype.intersection = function (set) {
	const result = new Set();
	for (const value of set) {
		if (this.has(value)) result.add(value);
	}
	return result;
};

// 합집합
Set.prototype.union = function (set) {
	const result = new Set(this);
	for (const value of set) {
		result.add(value);
	}
	return result;
};

// 차집합
Set.prototype.difference = function (set) {
	const result = new Set(this);
	for (const value of set){
		result.delete(value);
	}
	return result;
};

// 부분 집합과 상위 집합
Set.prototype.isSuperset = function (subset) {
	for (const value of subset) {
		if (!this.has(value)) return false;
	}
	return true;
};
```

## Map

Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다.

Map 객체는 객체와 유사하지만 다음과 같은 차이가 있다.

| 구분 | 객체 | Map 객체 |
| --- | --- | --- |
| 키로 사용할 수 있는 값 | 문자열 또는 심벌 값 | 객체를 포함한 모든 값 |
| 이터러블 | X | O |
| 요소 개수 확인 | Object.keys(obj).length | map.size |

### Map 객체의 생성

```jsx
const map = new Map();
console.log(map);  // Map(0) {}

// Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다.
// 이때 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.
const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map1);  // Map(2) {"key1" => "value1", "key2" => "value2"}

// Map 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어써진다.
const map = new Map([['key1', 'value1'], ['key1', 'value2']]);
console.log(map);  // Map(1) {"key1" => "value2"}
```

### 요소 개수 확인

```jsx
// Map 객체의 요소 개수를 확인할 때는 Map.prototype.size 프로퍼티를 사용한다.
const { size } = new Map([['key1', 'value1'], ['key1', 'value2']]);
console.log(size);  // 2
```

### 요소 추가

```jsx
// Map 객체에 요소를 추가할 때는 Map.prototype.set 메서드를 사용한다.
const map = new Map();
console.log(map);  // Map(0) {}

map.set('key1', 'value1');
console.log(map);  // Map(1) {"key1" => "value1"}

// Set의 add 메서드와 동일하게 set 메서드를 호출한 후에 set 메서드를 연속적으로 호출할 수 있고,
// 중복된 키를 갖는 요소를 추가하면 값이 덮어써진다.
```

### 요소 취득

```jsx
// Map 객체에서 특정 요소를 취득하려면 Map.prototype.get 메서드를 사용한다.
const map = new Map();
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');

console.log(map.get(lee));  // developer
console.log(map.get('key'));  // undefined
```

### 요소 존재 여부 확인

```jsx
// Map 객체에 특정 요소가 존재하는지 확인하려면 Map.prototype.has 메서드를 사용한다.
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

console.log(map.has(lee));  // true
console.log(map.has('key'));  // false
```

### 요소 삭제

```jsx
// Map 객체의 요소를 삭제하려면 Map.prototype.delete 메서드를 사용한다.
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };
const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.delete(kim);
console.log(map);  // Map(1) { {name: "Lee"} => "developer" }
```

### 요소 일괄 삭제

```jsx
// Map 객체의 요소를 일괄 삭제하려면 Map.prototype.clear 메서드를 사용한다.
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };
const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.clear();
console.log(map);  // Map(0) {}
```

### 요소 순회

Map 객체의 요소를 순회하려면 Map.prototype.forEach 메서드를 사용한다.

- 첫 번째 인수: 현재 순회 중인 요소값
- 두 번째 인수: 현재 순회 중인 요소값
- 세 번째 인수: 현재 순회중인 Map 객체 자체

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };
const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.forEach((v, k, map) => console.log(v, k, map));

// Map 객체는 이터러블이므로 for ... of 문, 스프레드 문법, 배열 디스트럭처링으로 순회할 수 있다.
for (const entry of map) {
	console.log(entry);
}

console.log([...map]);

const [a, b] = map;
console.log(a, b);
```
