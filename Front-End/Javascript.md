# Javascript

## Javascript Engine
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
