# React
[JSX](#JSX)<br />
[Virtual Dom](#Virtual-Dom)<br />
<br />

## JSX
JSX(Javascript eXtension) 은 자바스크립트의 확장문법이다. React는 HTML 요소를 표현하기 위해 JSX를 사용한다.<br />
마크업 언어처럼 보이나, 말 그대로 JSX는 Javascript 다. (Build 시, Babel에 의해 JS로 변환됨) 기본적으론 마크업 문법으로 작성할 수 있으며, 추가적인 문법이나 반드시 준수해야 할 주의점이 있다!<br />
- 반드시 하나의 부모 태그로 감싸준다. (태그로 특정할 필요가 없다면 빈 태그도 가능)
- 클래스명은 class가 아닌 className 으로 명명한다.
- 스타일 속성은 style={} 중괄호 안에 객체 형태로 포함시키며, 속성명은 카멜케이스로 적는다.
- 모든 태그는 반드시 클로징(" ... />") 되어야 한다. div와 같은 태그도 셀프 클로징 가능
- JSX의 {}(중괄호) 안에 Javascript 표현식을 사용할 수 있다.
- 주석은 // 도 가능하나, 기본적으론 {/* ~~ */} 형태이다.
<br />

## Virtual Dom
SPA(Single Page Application) 은 DOM 조작이 많이 발생한다. 이러한 변경점마다 페이지 전체의 렌더 트리를 다시 그리는 것은 비효율적이다. (jQuery가 그랬다)<br />
Virtual DOM은 DOM을 추상화한 가벼운 복사본으로, 메모리엔 저장되나 실제 렌더되지 않는다. 이 가상DOM에 변경점을 반영하고, 현재 DOM과 대조하여 변경된 컴포넌트만 리렌더해주는 것이다. 이 과정을 **재조정(Reconciliation)** 이라고 칭한다.<br />

이전의 전체 리렌더하는 방식에 비해 브라우저의 연산량을 줄여 페이지 로드를 최적화할 수 있다.<br />
[https://velog.io/@sbinha/React에서-Virtual-DOM](https://velog.io/@sbinha/React%EC%97%90%EC%84%9C-Virtual-DOM)

<br />

## key 사용이유
반복적인 엘리먼트에 안정적인 고유성 부여 → 순회적으로 렌더링을 처리하는데, 추가/수정/제거/순서변경 시 해당 포인트부터 리렌더링이 시작되므로 비효율적
- index를 키값으로 부여하면, 수정이 일어났을 때 key를 기준으로는 변경이 없다고 고려하여 리렌더링을 유발하지 않는다
<br />

## Suspense 원리
컴포넌트 렌더링에 필요한 대기 기능을 제공(fallback UI). 기존엔 Lazy Loading을 위한 실험적 기능이었으나, v18 정식출시 이후론 코드 스플리팅 목적뿐만 아니라 패칭 등 다양한 케이스를 대응한다. 
- 원리 : **Promise를 throw하여 상위에 비동기를 위임.** 감싸진 컴포넌트를 runPureTask 로직으로 구독하며, pending(throw Promise) / success(return result) / error(throw error)를 반환
