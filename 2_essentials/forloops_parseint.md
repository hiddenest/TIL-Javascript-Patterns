## 1. for loop

1. 일반적인 사용

```Javascript
for (let i = 0; i < myarray.length; i++) {
    // myarray[i]
}
```

루프를 돌 때마다 length를 구한다. 일반적 배열이 아니라 HTMLCollection일 경우, 코드가 느려질 수 있다.
* 그러나 ES6에서는 for 문보다 `map()`, `reduce()`, `forEach()`, `filter()`, `find()` 과 같이 명시적인 메소드를 쓰는 것을 권장한다.

- `map()` - 모두 어떤 연산 거쳐 나오도록
- `forEach()` - 파이썬의 for문 같은
- `filter()` - 조건에 해당하는거 모두
- `find()` - 조건에 해당하는거 하나
- `reduce()`

2. refactor 1 - 캐싱

```Javascript
for (let i = 0, max = myarray.length; i < max; i++) {
    // myarray[i]
}
```

length 값 한번만 구하고 캐싱된 max 만 사용한다. HTMLCollection 를 순회할 때 이렇게 쓰면 훨씬 속도가 향상된다.
* 그러나 콜렉션이 변경되는 경우에는 변경되는 length를 그때그때 순회하는게 맞다.

3. refactor 2 - 단일 var 패턴(변수 선언한 부분을 모아 놓는 방법)

```Javascript
let i = 0,
    max,
    myarray = [];

for (i = 0, max = myarray.length; i < max; i++) {
    // myarray[i]
}
```

4. refactor 3

JSLint 에서는 `i++` 보다 `i+=1`을 권장한다.

5. 변형 패턴 1 - descending for loop

```Javascript
let i, myarray = [];

for (i = myarray.length; i-=1;) {
    // myarray[i]
}
```

`myarray.length`에서 내려가는 방식. 변수도 줄이고, length와의 비교보다 0과의 비교가 대개 더 빠르다.

* `i-=1`도 일종의 조건문이다. 이게 음수로 내려가지 않는 이유는 자바스크립트에서는 0을 false로 인식하기 때문이다.

5. 변형 패턴 2 - while문

```Javascript
let myarray = [],
    i = myarray.length;

while (i--) {
    // myarray[i]
}
```

---

## 2. for-in loop

배열에서는 최대한 for loop를 사용하고 객체에서만 for-in loop를 사용하는 것이 바람직하다. 배열이 변경되었을 때, 오류가 발생할 수 있기 때문이다.

1. `hasOwnProperty()` 사용하기

```Javascript
let man = {
    hands: 2,
    legs: 2,
    heads: 1
};

if (typeof Object.prototype.clone === "undefined") {
    Object.prototype.clone = function () {};
}
```

* `Prototype`?

객체에 clone이라는 메소드를 추가했다. 이럴 때 프로퍼티를 검사하지 않고 순회할 경우, 아래처럼 property가 아닌 clone까지 출력하게 된다.

```Javascript
// Anti-pattern
for (let i in man) {
    console.log(i, man[i]);
}
// 결과
// hands 2
// legs 2
// heads 1
// clone function()
```

- `hasOwnProperty()`를 사용하여 프로토타입 프로퍼티를 걸러낼 경우

```Javascript
for (let i in man) {
    if (man.hasOwnProperty()) {
        console.log(i, man[i]);
    }
}
// 결과
// hands 2
// legs 2
// heads 1
```

- 또 다른 방법:

```Javascript
for (let i in man) {
    if (Object.prototype.hasOwnProperty.call(man, i)) {
        console.log(i, man[i]);
    }
}
```

```Javascript
let i,
    hasOwn = Object.prototype.hasOwnProperty;
for (i in man) {
    if (hasOwn.call(man, i)) {
        console.log(i, man[i]);
    }
}

// Refactored - for loop과 if문을 하나의 완결된 생각으로 묶기. but JSLint를 통과하지 못한다!
let i,
    hasOwn = Object.prototype.hasOwnProperty;
for (i in man) { if (hasOwn.call(man, i)) {
        console.log(i, man[i]);
    }
}
```

---

## 3. 내장 생성자 프로토타입 확장

생성자 함수의 prototype 프로퍼티를 확장하는 것은 바람직하지 않다. 다른 개발자가 이 확장된 코드를 알지 못하면 코드가 예측에서 벗어나는 일이 생기기 때문이다. 이미 있는 기능인지 살펴보거나 부득이 바꾸게 된다면 문서화 하는게 좋다.

---

## 4. switch 패턴

```Javascript
let inspect_me = 0,
    result = '';

switch (inspect_me) {
case 0:
    result = 'zero';
    break;
case 1:
    result = 'one';
    break;
default:
    result = 'unknown';
}
```

1. `case`문을 switch 문에 맞춰 정렬한다.
2. `case`문 안에서 들여쓰기 한다.
3. 각 `case`는 명확하게 `break;`로 종료한다.
4. 마지막에는 반드시 `default`를 지정한다. (* if, elif, elif... else!)

---

## 5. 암묵적인 타입캐스팅 피하기

1. `==` vs `===`

```Javascript
console.log(0 == false);
// true
console.log(0 === false);
// false
```

2. '`eval()` is evil!'

`eval()`은 임의의 문자열은 자바스크립트 코드로 실행한다.

```Javascript
// Anti-pattern
let property = "name";
alert(eval("obj." + property));

// 권장
let property = "name";
alert(obj[property]);
```

마찬가지 이유로 `setInterval()`, `setTimeout()`, `Function()` 생성자에 문자열을 넘기는 것도 좋지 않다.

```Javascript
// Anti-pattern
setTimeout("myFunc()", 1000)
setTimeout("myFunc(1,2,3)", 1000)

// 권장
setTimeout(myFunc, 1000)
setTimeout(function () {
    myFunc(1,2,3);
}, 1000);
```

---

## 6. `parseInt()`를 통한 숫자 변환

`parseInt()`를 사용할 땐 기수를 명시하는 것을 기억해야 한다.

```Javascript
let month = '06',
    year = '09';

month = parseInt(month, 10);
year = parseInt(year, 10);
```

* '06'과 같이 쓰여있을 경우, parseInt(year, 10)는 기수가 8인 것으로 간주된다.

Number()를 통해서 변환도 가능하고 이는 parseInt() 보다 빠르지만 '08 hello'와 같은 값을 변환하지 못한다.

---
