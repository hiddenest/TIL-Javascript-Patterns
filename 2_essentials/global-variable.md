# 전역변수

자바스크립트는 함수 단위 스코프이다.  
모든 자바스크립트의 런타임에는 전역 객체가 존재한다. (브라우저는 window, Node.js는 global 등)  

어떤 함수에 속하지 않은 상태에서 `this`를 사용 할 경우에는 전역 객체에 접근하게 된다.  
전역 변수를 생성하는 것은 전역 객체의 프로퍼티를 만드는 것과 같다.   
```javascript 
//browser
hello = 'hello world';
console.log(hello); // 'hello world'
console.log(this.hello); // 'hello world'
console.log(window.hello); // 'hello world'
console.log(window['hello']); // 'hello world'
  
//NodeJS
hello = 'hello world';
console.log(hello); // 'hello world'
console.log(this.hello); // 'hello world'
console.log(global.hello); // 'hello world'
console.log(global['hello']); // 'hello world'
```

전역 변수의 문제점은 다음과 같다. 
- Javascript 어플리케이션 (웹 페이지, 서버 등) 내 모든 코드 사이에서 공유된다.
- 서드파티 라이브러리와 충돌이 있을수 있다.

다음과 같은 코드를 짜면 전역 변수가 생성된다.
```javascript
function sum(x,y){
  result = x + y;
  return result;
}
  
sum(10, 20); // 30
console.log(result) // 30
```
위 코드에서 result는 선언해주지 않고 사용하였기 때문에 전역 객체에 프로퍼트로 정의가 된다.  
그래서 위 코드를 개선시키면 아래와 같다.

```javascript
function sum(x,y){
  var result = x + y;
  return result;
}
```

그리고 다음과 같은 경우에도 전역 객체에 변수가 선언된다.
```javascript
function sameAs10(){
  var a = b = 10;
  // ...
}
  
sameAs10();
  
console.log(this.a) // undefined
console.log(this.b) // 10
```
위 코드에서 a는 변수 선언을 하고 값을 대입했지만 b의 경우에는 선언되지 않은 상태에서 대입을 하였기 때문에 전역 객체의 프로퍼티로 정의되었다.  
    
위 코드는 다음과 같이 개선 할 수 있다.
```javascript
function sameAs10() {
  var a,b;
  a = b = 10;
}
  
sameAs10();
  
console.log(this.a) // undefined
console.log(this.b) // undefined
```  

strict 모드에서는 선언되지 않은 변수에 값을 할당하면 에러가 발생한다.

함수가 new와 생성자를 사용해 호출되지 않고 그냥 함수로 호출되는 경우에는 this가 항상 전역 객체를 가르킨다
하지만 strict 모드에서는 적용되지 않는다.

### 단일 var 패턴
`단일 var 패턴`은 함수 상단에서 var 선언을 한 번만 쓰는 패턴을 말한다.  
단일 var 패턴을 사용했을 경우에는 다음과 같은 이점들이 있다.
- 함수에서 필요로 하는 모든 지역 변수를 한군데서 찾을 수 있다.
- 변수를 선언하기 전에 사용할 때 발생하는 로직상의 오류를 막아준다.
- 변수를 먼저 선언한 후에 사용해야 한다는 사실을 상기시키기 때문에 전역 변수를 최소화하는 데 도움이 된다.
- 코드량이 줄어든다.

#### Example
```javascript
function doSomething(){
  var a = 10,
      hello = 'Hello world',
      obj = {};
  
  // ...
}
```
 