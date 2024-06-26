# Javascript
[var, let, const](#%EF%B8%8F-var,-let,-const)<br />
[Hoisting](#%EF%B8%8F-Hoisting)<br />
[Javascript 엔진](#%EF%B8%8F-Javascript-엔진)<br />
[비동기 프로그래밍](#%EF%B8%8F-비동기-프로그래밍)<br />
[Prototype](#%EF%B8%8F-Prototype)<br />
[Arrow Function(화살표 함수)](#%EF%B8%8F-Arrow-Function(화살표-함수))<br />
[Closure(클로저)](#%EF%B8%8F-Closure(클로저))<br />
[Garbage Collection(가비지 컬렉션)](#%EF%B8%8F-Garbage-Collection(가비지-컬렉션))<br />
[this 바인딩](#%EF%B8%8F-this-바인딩)<br />
[실행 컨텍스트](#%EF%B8%8F-실행-컨텍스트)<br />
[함수 Composition](#%EF%B8%8F-함수-Composition)<br />
<br />

## ✏️ var, let, const
var는 ES5까지, let/const는 ES6에 등장한 변수선언 키워드다.
1. var는 함수레벨 스코프, let/const는 블록레벨 스코프
2. var는 선언+초기화 호이스팅, let/const는 선언만 호이스팅(TDZ)
3. var는 재선언 가능, let/const는 재선언 불가능
4. var와 let은 재할당 가능, const는 재할당 불가능
5. (strict mode가 아닌 경우) 전역 스코프에서 var만 window에 바인딩
<br />

## ✏️ Hoisting
Javascript에서 호이스팅은 코드에 선언된 **변수(var) 및 함수 선언문이 해당 스코프의 최상단**으로 끌어올려지는 것이다. (함수범위 혹은 전역범위)
컴파일러는 자바스크립트 엔진이 인터프리팅(interpreting) 하기 전에 컴파일을 하는데, 이 때 호이스팅이 발생하는 것이다.

### 호이스팅의 대상
var 변수 선언과 함수 선언문에서만 호이스팅이 일어난다. (할당은 호이스팅되지 않음)
```
// 이 코드는
console.log(a);
var a = 1;
console.log(a);

// 이처럼 실행된다.
var a;	// 변수 선언부분만 끌어올려졌다.
console.log(a);	
a = 1;
console.log(a);	
```

```jsx
function foo() { // 함수선언문
  console.log("hello");
}
var foo2 = function() { // 함수표현식
  console.log("hello2");
}
```
### TDZ(Temporal Dead Zone)
const, let, 클래스 등 모든 선언에 대해 호이스팅은 작동한다.
단, var는 선언+초기화가 호이스팅되어 undefined, const/let은 선언만 호이스팅되어 ReferenceError 로 각각 찍히는 것이다.
- 선언 : 사용할 변수나 함수 등 변수명을 선언하는 단계. 실행 컨텍스트에서 진행된다.
- 초기화 : 선언된 변수에 **Call Stack 메모리를 할당하고, 주소값을 실행 컨텍스트 변수에 저장하고, 암묵적으로 undefined로 초기화**하는 단계. => 여기서 var는 초기화되어 호이스팅, const/let은 선언만 실행되는 것
- 할당 : 런타임 단계에서 `=(할당연산자)`를 만나면, 메모리에 값을 교체해주는 단계.

### 호이스팅의 규칙
- 함수선언문은 구현위치와 관계없이 맨 위로 호이스팅된다.
- 함수표현식은 선언과 할당이 분리된다. (var 표현식은 선언부만 호이스팅)
- new Function() 객체 역시 호이스팅되지 않는다.
- 같은 이름의 변수 선언문은 함수 선언문보다 위로 호이스팅된다.
- 값이 할당된 변수는 변수가 함수선언문을, 할당되지 않은 변수는 함수선언문이 변수를 덮어쓴다. (즉, 비어있다면 함수가 들어오게 됨)
```
var myName = "Heee"; // 값 할당
var yourName; // 값 할당 X

function myName() { // 같은 이름의 함수 선언
	console.log("myName Function");
}
function yourName() { // 같은 이름의 함수 선언
	console.log("yourName Function");
}

console.log(typeof myName); // > "string"
console.log(typeof yourName); // > "function"
```
<br />
<br />

## ✏️ Javascript 엔진
자바스크립트 엔진은 자바스크립트 코드를 해석하고 실행하는 인터프리터이다. 브라우저마다 다른 엔진을 사용한다.
- `V8` : 크롬, Node.js
- `SpiderMonkey` : 파이어폭스
- `Chakra` : 익스플로러, 엣지
- `Webkit` : 사파리

### 자바스크립트 엔진 구조(런타임)
<img src="https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/062d1ebc-3564-4dbd-b7f7-d4385e67a98a" width="600" /><br />
- `Memory Heap` : 참조타입(배열, 객체, 함수선언) 값이 무작위로 저장되는 곳
- `Call Stack`: 함수 호출 시 **실행 컨텍스트 스택**이 쌓이는 곳, 원시타입 값도 저장
- `Callback Queue(Event Queue)` : setTimeout 과 같은 비동기 콜백이 저장되는 메모리
- `Web API` : 브라우저에서 제공하는 API들. 실행되면 콜스택에서 background로 이동된다.
<br />

### Call Stack(콜스택)
자바스크립트는 싱글 쓰레드 언어이기 때문에, 한 번에 한 가지 작업만 실행한다. 콜스택은 함수가 실행됬을 때 쌓이고(push), 종료되면 Pop() 되는 곳이다. 호출 스택의 각 항목을 스택 프레임 이라고 칭한다.<br />
*stack overflow: 스택의 크기를 초과하면 "Maximum call stack size exceeded" 에러와 함께 함수를 종료시킨다.

### Event Loop(이벤트 루프)
콜스택에서 작업이 오래걸리는 서버통신, 비동기 등을 동기로 처리하면 다른 작업들이 대기상태가 되고, 이것은 매우 비효율적일 것이다.<br />
이러한 상황을 제어하는 것이 비동기 콜백이다. 즉, 위에 해당하는 로직을 별도로 저장(콜백큐)한 다음, 콜스택의 동기로직들이 완료되어 비었을 때 인입하여 실행시키는 것이다.<br />
이러한 콜백함수들의 스케쥴을 관리하는 것이 Event Queue 이다.<br />
<br />

### Event Queue(이벤트 큐, Callback Queue)
<img src="https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/c859bb84-5c16-4dbd-b186-b8dce51da7a6" width="600" /><br />
자바스크립트 런타임은 콜백큐를 가지고 있다. 이는 처리할 메세지 목록과 실행할 콜백함수들의 리스트이다.<br />
Web API 들이 콜스택에 올라오면, background를 통해 제어된 다음 내부의 로직(혹은 콜백함수)을 이벤트 큐로 이동시킨다. 이후, 콜스택이 빌 때 이 로직들이 인입되어 실행되는 것이다!<br />
[https://velog.io/@ru_bryunak/자바스크립트-기초-1](https://velog.io/@ru_bryunak/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B8%B0%EC%B4%88-1)
<br />
<br />

## ✏️ 비동기 프로그래밍
Javascript는 싱글 스레드로 한 번에 하나의 작업만 수행 가능하다. 이 떄, 연산량이 높은 작업으로 다음 작업이 지연되는 것을 방지하기 위해 JS는 비동기(Asynchronous) 개념을 제공한다.

### 비동기 원리
<img src="https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/e3346c51-a1d6-4bd8-947a-52619b828d4b" width="500" />
1. 콜스택 작업을 실행하던 중 비동기 함수를 만난다. (fetch, setTimeout, ajax, eventListener 등)
2. Web APIs에 비동기를 위임하고 다음 작업을 지속한다. Web APIs는 멀티 스레드로, 백그라운드에서 작업을 처리한다.
3. 백그라운드 작업이 완료되면 이후의 콜백함수를 콜백큐에 넣는다. 이벤트 루프에서 콜스택이 비어있다면 콜백큐로부터 작업을 가져와 실행한다.
<br />

### 비동기 스케줄링
한 작업을 비동기 처리했다고 가정했을 때, 다음 작업이 그 결과값을 요구한다면 문제가 생긴다. 이 때문에, 비동기 스케줄링 기법들이 등장하게 된 것이다.
1. 콜백함수(Callback Function) : 특정 시점에 호출하는 함수. JS에서의 1급함수 성질을 활용해, 함수를 매개변수를 받아 이를 비동기 작업 이후 실행
2. Promise 객체 : Javascript 비동기 전용 객체. 비동기 결과값뿐만 아니라, 상태 및 성공/실패 여부를 제공해 깔끔한 체이닝이 가능
   - Promise 상태 : pending, fulfilled, rejected, settled
   - Promise 메서드 : then, catch, finally, all, race, resolve, reject 등
3. async/await : Promise 체이닝에서 가독성을 높인 문법. async 함수는 Promise를 반환하며, 내부 await 키워드는 Promise가 resolve 될 때까지 대기 후 실행된다.
<br />
<br />

## ✏️ Prototype
**Javascript는 프로토타입 기반 객체지향 언어**이다. 이는, 원형 객체를 복제하여 새로운 객체를 생성하는 원리이다. 다만, 자바스크립트는 실제 복제를 하지 않고 프로토타입 링크(__proto__) 를 통해 원형을 참조한다.
* C, Java는 클래스 기반 객체지향 언어. 클래스라는 추상화된 개념을 상속한 새로운 객체(인스턴스)로 생성하는 원리

### 프로토타입 링크
<img src="https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/d9a7c16e-8c2b-4fdd-a1e4-0ab10fc064ce" width="600" /><br />
Javascript 에서 원시타입 값을 제외한 모든 타입은 객체이다. 그렇기에, 모든 객체는 원형 객체 및 이에 대한 링크(__proto__)를 가지며, 이를 타고 올라가면 Object() 에 도달하는데 이것이 프로토타입 링크이다.

### 주요 속성
<img src="https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/c5518969-3f15-47b8-ab49-4106031ececc" width="600" /><br />

**1. Prototype Object**
자바스크립트에서 객체는 항상 함수로만 생성된다. 이 함수는, 생성자(Constructor) 자격을 가지며 new 키워드로 객체를 만들어낼 수 있다.
새로운 객체를 만들 때, 함수는 Prototype Object를 같이 만들며, 함수의 prototype 속성을 통해 이 객체에 접근할 수 있다. 또한, 함수가 만드는 새로운 객체들은 바로 이 Prototype Object를 참조하는 것이다.
- [[prototype]] 속성 : 모든 객체가 갖는 필드. 부모 역할을 하는 프로토타입 객체 정보를 담고 있음.
- prototype 속성 : 함수(생성자)만 가지는 필드. 생성자 함수로 만들어진 객체의 부모 프로토타입 객체를 가리킴.
- constructor : Prototype Object 가 가지는 필드. 생성자 함수를 가리키며, 이 자격으로 새로운 객체를 만들 수 있다.

**2. Prototype Link**
모든 객체가 갖는 필드이다. __proto__ 필드명으로 확인할 수 있으며, 일종의 링크이다. 이 링크를 통해, 원형 객체([[prototype]]) 와 연결될 수 있다.

[https://abangpa1ace.tistory.com/104?category=910462](https://abangpa1ace.tistory.com/104?category=910462)https://abangpa1ace.tistory.com/104?category=910462
<br />
<br />

## Arrow Function(화살표 함수)
Arrow Function은 ES6에 새로 추가된 문법으로, 비교적 간단하게 함수를 선언할 수 있다. 무조건 익명이므로, 함수를 호출하기 위해선 함수 표현식으로 변수에 저장해야한다.
```
(x, y) => x + y  // = const foo = function(x,y) { return x + y; };
```
### 기존 함수선언과 차이점
- 콜백함수 등을 작성할 때 매우 간결해진다.
- 본인을 포함하는 외부 스코프에서 this를 계승받는다. 자신만의 this 바인딩을 하지 않는다. (Lexical Scope 를 따름)

### 사용하면 안되는 경우
대부분 사용 가능하나 this와 긴밀한 제어가 필요한 로직에 주의!
- ES6 문법으로, 구형 브라우저(IE11 등) 에서 사용할 수 없다.
- 객체의 메서드로 사용할 경우, this가 객체를 바인딩하지 않고 사용 스코프의 상위를 바인딩하기 때문에 적절하지 않다.
- 이벤트 리스너의 콜백함수로 사용하면 this가 상위 컨텍스트(window)를 가리킴
- 생성자 함수로 사용할 수 없다. (prototype 이 없음)
<br />
<br />

## ✏️ Closure(클로저)
클로저는 함수가 선언될 때 속한 렉시컬 스코프(Lexical Environment)를 기억하여, 스코프 밖에서 실행될 때도 이 스코프에 접근할 수 있게 해주는 기능이다.
```
function sayHello () {
  const a = 'Hello';
  const b = 'World';
  
  function sumString () {
    console.log(a + ' ' + b);
  }
  
  return sumString;
}

const myFunc = sayHello();

myFunc(); // 'Hello World'
```
외부함수(sayHello) 실행이 끝나고 소멸된 후에도, 내부함수(sumString)가 외부함수 변수(렉시컬 환경)에 접근 가능하다는 예시코드다.

### Closure 사용하는 이유
1. 상태 유지 (debounce 함수에서 timer를 기억 등)
2. 정보 은닉 (변수값을 렉시컬 스코프에 은닉시키고, private 메서드를 모방할 수 있음)
3. 전역 변수 사용 억제

### Closure 주의점
메모리 측면에서 손해(내부함수가 외부함수를 참조중이어서, GC로 메모리가 해제되지 않음) => 클로저를 할당한 변수에 null을 할당하여 해제
<br />
<br />

## ✏️ Garbage Collection(가비지 컬렉션)
https://fe-developers.kakaoent.com/2022/220519-garbage-collection/
<br />
<br />

## This 바인딩
### 기본 바인딩
자바스크립트는 기본적으로 전역객체에 컨텍스트가 바인딩된다. (global, window) 하지만, strict mode 에서는 undefined이다.

### 암시적 바인딩
함수 호출시 객체의 프로퍼티로 접근해서 실행하는 암시적 바인딩이다. 기본적으로 해당 객체에 바인딩되나, 전역에 레퍼런스를 저장하는 순간 전역 scope를 참조하게 된다

```
function hello() {
  console.log(this.name)
}

var obj = {
  name: "chris",
  hello: hello,
}

obj.hello() // 'chris'
```

### 명시적 바인딩
직관적으로 객체를 컨텍스트에 바인딩하는 방법이다. 자바스크립트의 call(), apply(), bind() 내장함수들이 다음 역할을 수행한다. (빈 객체를 넘기는 경우, 자동적으로 전역에 바인딩된다)

- **call(), apply()**

```
function hello() {
  console.log(this.name)
}

var obj = {
  name: "chris",
}

name = "global context!"
hello.call(obj) // "chris"
```

apply() 도 call() 과 같이 함수호출에 제공되는 this 값을 첫 번째 인자로 전달한다. 차이점은 두 번째 매개변수부터 call() 은 각각, apply() 는 배열로 받는다는 차이점이다.

- **bind()**

```
function hello() {
  console.log(this.name)
}

var obj = {
  name: "chris",
}

setTimeout(obj.hello.bind(obj), 1000) // 1초 후에 hello 함수가 동작하면 this는?

name = "global context!"
```

bind()는 함수를 정의할 때, 컨텍스트를 바인딩한다. (하드 바인딩) 즉, call(), apply() 처럼 함수를 실행하는 기능이 아닌, 미리 바인딩하는 목적이 있는 것이다.

### New 바인딩

```
function Person(name) {
  this.name = name
}
Person.prototype.hello() {
  console.log(this.name)
}

var obj = new Person('chris');
obj.hello(); // "chris"
```

함수를 new 키워드로 호출하면, 새로운 객체(프로토타입)를 반환하여 이것이 변수에 할당된다. 즉, 이 변수(obj)에 대해 함수를 실행하면 전역이 아닌 해당 객체가 this와 바인딩되는 규칙을 따른다.

**우선순위: New 바인딩 > 명시적 바인딩(내장함수) > 암시적 바인딩(객체 메서드) > 전역**
<br />

## ✏️ 실행 컨텍스트
Javascript가 코드들을 실행하기 위해 일종의 블록으로 나누고, 코드블록엔 변수, 함수, this, arguments 등 정보가 담긴다.<br />
실행가능한 코드를 추상화한 환경을 실행 컨텍스트라고 한다. 실행 컨텍스트는 Global, 함수, eval 3가지 경우에 생성되며, 콜스택에 쌓인다.

### ES6의 실행 컨텍스트
ES5까지는 변수객체, scope체인, this바인딩 으로 구성된 실행 컨텍스트를 채택했으나, ES6부터는 구성요소가 달라졌다.<br />
![image](https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/d8082f84-d169-40e2-a1d9-bc26efb75a81)
1. Lexical Environment : Environment Records(let/const 변수, 함수 관련값들을 추적), scope(Outer Reference Environment), this 바인딩 등으로 구성된다.
2. Variable Environment : LE와 동일하되 var 변수를 핸들링한다. var는 functional scope이며, 변수 초기선언이 달라서 별도 환경에 존재한다.
<br />

## ✏️ 함수 Composition
함수들을 조합하여 새로운 함수를 만드는 것. 함수를 겹겹이 실행하거나, 메서드 체이닝, 혹은 아래처럼 직접 구현할 수 있다.
```
const pipe = (...funcs) => (initialVal) => funcs.reduce((val, fn) => fn(val), initialVal);
```
<br />

### 커링(Currying)
고차함수에 기반한 프로그래밍 기법. 여러 인수를 받는 함수를, 각각의 인수를 받는 함수들로 나누는 방법입니다.<br />
기능이 복잡한 함수형 프로그래밍에서, 이 비용을 분할하며 재사용성을 강화하기 위해 사용되는 기법입니다.
```
// 기본함수
function add (a, b) {
  return a + b;
}

// 커링된 함수
var add = function(x) {
  return function(y) { 
    return x + y
  };
};

var addTen = add(10);
```
