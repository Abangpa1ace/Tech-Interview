# Javascript
[Hoisting](#Hoisting)<br />
[Javascript 엔진](#Javascript-엔진)<br />
[Prototype](#Prototype)<br />
<br />

## Hoisting
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

## Javascript 엔진
자바스크립트 엔진은 자바스크립트 코드를 해석하고 실행하는 인터프리터이다. 브라우저마다 다른 엔진을 사용한다.
- `V8` : 크롬, Node.js
- `SpiderMonkey` : 파이어폭스
- `Chakra` : 익스플로러, 엣지
- `Webkit` : 사파리

### 자바스크립트 엔진 구조(런타임)
![image](https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/062d1ebc-3564-4dbd-b7f7-d4385e67a98a)
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
![image](https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/c859bb84-5c16-4dbd-b186-b8dce51da7a6)
자바스크립트 런타임은 콜백큐를 가지고 있다. 이는 처리할 메세지 목록과 실행할 콜백함수들의 리스트이다.<br />
Web API 들이 콜스택에 올라오면, background를 통해 제어된 다음 내부의 로직(혹은 콜백함수)을 이벤트 큐로 이동시킨다. 이후, 콜스택이 빌 때 이 로직들이 인입되어 실행되는 것이다!<br />
[https://velog.io/@ru_bryunak/자바스크립트-기초-1](https://velog.io/@ru_bryunak/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B8%B0%EC%B4%88-1)
<br />

## Prototype
**Javascript는 프로토타입 기반 객체지향 언어**이다. 이는, 원형 객체를 복제하여 새로운 객체를 생성하는 원리이다. 다만, 자바스크립트는 실제 복제를 하지 않고 프로토타입 링크(__proto__) 를 통해 원형을 참조한다.
* C, Java는 클래스 기반 객체지향 언어. 클래스라는 추상화된 개념을 상속한 새로운 객체(인스턴스)로 생성하는 원리

### 프로토타입 링크
![image](https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/d9a7c16e-8c2b-4fdd-a1e4-0ab10fc064ce)
Javascript 에서 원시타입 값을 제외한 모든 타입은 객체이다. 그렇기에, 모든 객체는 원형 객체 및 이에 대한 링크(__proto__)를 가지며, 이를 타고 올라가면 Object() 에 도달하는데 이것이 프로토타입 링크이다.

### 주요 속성
![image](https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/c5518969-3f15-47b8-ab49-4106031ececc)

**1. Prototype Object**
자바스크립트에서 객체는 항상 함수로만 생성된다. 이 함수는, 생성자(Constructor) 자격을 가지며 new 키워드로 객체를 만들어낼 수 있다.
새로운 객체를 만들 때, 함수는 Prototype Object를 같이 만들며, 함수의 prototype 속성을 통해 이 객체에 접근할 수 있다. 또한, 함수가 만드는 새로운 객체들은 바로 이 Prototype Object를 참조하는 것이다.
- [[prototype]] 속성 : 모든 객체가 갖는 필드. 부모 역할을 하는 프로토타입 객체 정보를 담고 있음.
- prototype 속성 : 함수(생성자)만 가지는 필드. 생성자 함수로 만들어진 객체의 부모 프로토타입 객체를 가리킴.
- constructor : Prototype Object 가 가지는 필드. 생성자 함수를 가리키며, 이 자격으로 새로운 객체를 만들 수 있다.

**2. Prototype Link**
모든 객체가 갖는 필드이다. __proto__ 필드명으로 확인할 수 있으며, 일종의 링크이다. 이 링크를 통해, 원형 객체([[prototype]]) 와 연결될 수 있다.

[https://abangpa1ace.tistory.com/104?category=910462](https://abangpa1ace.tistory.com/104?category=910462)https://abangpa1ace.tistory.com/104?category=910462
