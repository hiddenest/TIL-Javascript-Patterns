함수는 객체다. 변수에 할당할 수도 있고 다른 함수에 인자로 전달할 수도 있다. 인자가 되는 함수를 콜백함수, 또는 콜백이라고 부른다.

## 1. 콜백이란?

```Javascript
function writeCode(callback) {
    //
    callback()
    //
}

function introduceBugs() {
    //
}

writeCode(introduceBugs)
```

- 함수의 ()를 붙이면 즉시 실행, 붙이지 않으면 참조만 하고, 바깥 함수에서 알맞은 때에 실행해준다.

## 2. 콜백 예제

## 3. 콜백과 유효범위

## 4. 비동기 이벤트 리스너

## 5. 타임아웃

## 6. 라이브러리에서의 콜백
