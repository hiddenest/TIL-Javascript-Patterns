## 3. 배열 리터럴

1. 배열 생성하기

배열도 객체와 마찬가지로 내장 생성자 Array()를 사용하기보다 `let a = []`와 같이 생성해주는 것이 좋다. 아래 두개의 코드는 다른 방식으로 작동한다.

```JavaScript
let a = [3]
// a.length = 1
// a[0] = 3

let a = new Array(3)
// a.length = 3
// typeof a[0] = "undefined"
```

위의 코드는 3이라는 하나의 숫자를 가진 배열이 되고, 아래는 3개의 자리를 가진 빈 배열을 만들게 된다.

2. 배열 판별하기

왜 배열은 `typeof`가 아니라 `isArray()`로 판별할까? array도 객체이기 때문에 typeof는 "object"를 반환하기 때문이다.

---

## 4. JSON

JSON(JavaScript Object Notation)은 객체 리터럴 표기법으로 쓰여진 데이터 전송 형식이다.

1. `JSON.stringify(x)` - JSON은 프로퍼티명이 항상 문자열이어야 한다. 그래서 stringify로 모든 키값을 문자열로 serialize하는 게 안전하다.
2. `JSON.parse(x)` - JSON 데이터를 서버에서 던져줄 때 서버에서 stringify를 하고 프런트에서 파싱해서 쓰면 용량을 적게 사용할 수 있다.


---
