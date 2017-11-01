## 1. 객체 생성하기

```Javascript
// 1. Object 내장 생성자로 생성 - 안티패턴
a = new Object()

// 2. 객체 리터럴로 생성
a = {}
```

- 이런식으로 자바스크립트 내부의 Object, Array 같은 객체를 사용하기 보다는 리터럴을 사용하는 것이 좋다.
- 두번째 방법은 빈 객체로 보이지만 사실은 자바스크립트에는 빈 객체는 없다. 이렇게 생성하기만해도 `Object.prototype` 에서 상속받은 프로퍼티와 메서드를 가진다.

```Javascript
a = {}
a.name = 'Benji'    // 프로퍼티 추가
a.getName = function() {    // 메소드 추가
    return a.name
}

a.name = 'Fido' // 프로퍼티 수정

delete a.name   // 프로퍼티 삭제
```

- 이렇게 프로퍼티와 메소드를 추가/수정하거나 삭제할 수 있다.

---

## 2. 사용자 정의 생성자 함수

1. 객체 생성

```Javascript
let Person = function(name) {
    this.name = name
    this.say = function() {
        return "I am " + this.name
    }
}

let person = new Person('adam')
```

1. `Person`은 사실 함수일 뿐이지만 객체와 같은 역할을 하고 있다.
2. `new`는 새로운 객체를 생성하는 연산자이다.
    - `new`는 `this` 포인터를 만들고 이는 새로 만든 객체를 가리키게 된다.
    - 만약에 new 없이 `let person = Person('adam')`과 같이 그냥 함수만 호출한다면 함수가 실행되기만 할 뿐, `this` 포인터가 객체를 가리키지 못한다. 그래서 `Person`함수의 `this.name = name` 이 부분의 `this`는 전역을 가리키게 되고 `name`과 `say()`는 전역 변수, 전역 메소드로 세팅이 된다. 매우 위험한 상황이 되는 셈이다.
    - `new`를 쓰면 위의 코드의 이면에 진행되는 것은 아래 주석과 같다.

```Javascript
let Person = function(name) {
    // let this = {}
    this.name = name
    this.say = function() {
        return "I am " + this.name
    }
    // return this
}

let person = new Person('adam')
```

- 함수 내에 return문이 없더라도 생성자는 암묵적으로 this를 리턴한다. return문을 써서 반환되는 객체를 지정해줄수도 있다.

2. 메소드 추가/수정

위의 방법으로 메소드를 추가하는 방법도 있지만, 새로운 객체를 생성할 때마다 say() 라는 함수를 계속 만들게된다. 즉, n개의 객체를 만들면 n개의 say()가 만들어지는 것이다. 그러기보다 Person의 프로토타입에 say() 메소드를 추가하면 한번만 정의하고도 모든 객체가 참조할 수 있다.

```Javascript
Person.prototype.say = function() {
    return "I am " + this.name
}
```

** 프로토타입에 메소드를 추가했을 때 콘솔에 찍히는 내용

![prototype](/images/object_prototype.png)

** 보너스

```Javascript
function Waffle() {
    if (!(this isinstanceof Waffle)) {
        return new Waffle()
    }

    this.tastes = "yummy!"
}

Waffle.prototype.wantAnother = true
```

- new를 쓰지 않았을 때에도 위에 문제가 발생하지 않도록 강제하는 방법도 있다. 어떤 변수에 new없이 Waffle()을 할당해서 함수가 실행이 되었는데 그 실행된 함수안의 this가 Waffle의 인스턴스가 아닐 때, 즉 전역을 가리킬 때 그 함수 안에서 `new Waffle()`로 새로운 Waffle객체를 생성하는 것이다.

---
