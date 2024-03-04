# React
[JSX](#JSX)<br />
[Virtual Dom](#Virtual-Dom)<br />
[useEffect](#useEffect)<br />
[key 사용이유](#key-사용이유)<br />
[Babel, Webpack](#Babel,-Webpack)<br />
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

## useEffect
useEffect 훅은 함수 컴포넌트가 마운트 됐을 때, 언마운트 됐을 때, 업데이트 될 때 등, 생명주기와 비슷한 관점으로 특정 작업을 처리하기 위해 사용된다.(첫 번째 인자는 Effect 함수, 두 번째 인자는 Dependency Array)<br />
즉, 생명주기에 있다기 보다는 단순히 렌더링 후 사이드 이펙트를 실행하는 원리인 것이다.<br />
Effect 함수가 반환하는 함수를 clean-up 함수라고 하며 언마운트 시 실행된다.(컴포넌트 unmount ➡ cleanup 함수 실행 ➡ 컴포넌트 mount ➡ effect 실행)<br />

**useEffect 더 잘 쓰는 법**
- 단일 목적의 useEffect
- Custom Hook 화를 적극적으로
- 불필요한 Effect 로직은 조기에 return
- Effect에 관련된 모든 변수는 디펜던시에 추가
<br />

## key 사용이유
반복적인 엘리먼트에 안정적인 고유성 부여
- 순회적으로 렌더링을 처리하는데, 추가/수정/제거/순서변경 시 해당 포인트부터 리렌더링이 시작되므로 비효율적
- index를 키값으로 부여하면, 수정이 일어났을 때 key를 기준으로는 변경이 없다고 고려하여 리렌더링을 유발하지 않는다
<br />

## Babel, Webpack
### Babel: ES6를 지원하지 않는 브라우저에 ES5로 트랜스컴파일

**트랜스파일링**
트랜스파일링(Transpiling)이란 특정 언어로 작성한 코드를 비슷한 다른 언어로 변환시키는 행위이다. 이를 처리하는 툴이 트랜스파일러(Transpiler) 인 것이다.
트랜스파일러가 필요한 이유는, 모든 브라우저가 ES6를 지원하지 않기 때문에 이를 ES5로 변환시키는 과정이 필요하다.

이외에도, React JSX를 자바스크립트 코드로 변환시키거나, TS를 JS로 변환시키는 등의 기능도 담당한다. (전자는 Babel, 후자는 TS 트랜스파일러)
통상 프론트엔드 프레임워크는 모듈 번들러에 트랜스파일러를 추가하는 방식을 적용
<br />

### Webpack: 자바스크립트 정적 모듈 번들러

**모듈 번들러?**
현대 프론트엔드 개발은 모듈단위로 파일을 엮어서 개발하며, 각 모듈은 서로 의존성을 가진다. 모듈로 개발을 했을 때, 아래와 같은 문제점들이 발생한다.
- 수만은 모듈들의 순서를 어떻게 처리하나?(의존성)
- 모듈이 많아질수록 HTTP 요청이 많아질텐데 오버헤드를 어떻게 처리하나?
- ES6 이상 스펙의 코드를 어떻게 처리하나?

이를 해결하기 위한 것이 모듈 번들러(Module Bundler)로, 각각의 모듈 의존성을 해결하여 하나의 자바스크립트 파일로 만들어준다.
ES5 지원, 이미지 압축, 최소화 등 여러가지 기능들도 제공한다. 대표적인 예로, Webpack, Parcel, Rollup 등
<br />

## Suspense 원리
컴포넌트 렌더링에 필요한 대기 기능을 제공(fallback UI). 기존엔 Lazy Loading을 위한 실험적 기능이었으나, v18 정식출시 이후론 코드 스플리팅 목적뿐만 아니라 패칭 등 다양한 케이스를 대응한다. 
- 원리 : **Promise를 throw하여 상위에 비동기를 위임.** 감싸진 컴포넌트를 runPureTask 로직으로 구독하며, pending(throw Promise) / success(return result) / error(throw error)를 반환
<br />

## 단방향 데이터 바인딩
React는 단방향 데이터 바인딩 방식을 채택한다. 
**데이터 바인딩이란?** 두 데이터 혹은 정보의 소스를 일치시키는 기법. 즉, 화면의 데이터와 브라우저 메모리의 데이터를 일치시키는 기법이다.

**[양방향 바인딩]**
![image](https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/70e5fe9a-2bc4-40c4-93d2-608f9c37d8c2)
Vue, Angular 는 양방향 바인딩을 채택한다. UI 및 JS 양쪽의 Watcher가 데이터를 동기화시켜주는 것이다. 이외 부가적인 로직이 불필요한 장점이 있지만, 데이터가 많아지면 Watcher도 증가하여 성능저하의 단점이 있다.
<br />

**[단방향 바인딩]**
![image](https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/024b6582-294d-410a-8f7c-9ccc4b9ea32b)
React는 단방향 바인딩을 채택한다. 하나의 Watcher가 JS 데이터 갱신을 감지하여 UI를 업데이트한다. 반대로, 사용자가 상태값(혹은 JS)을 업데이트하기 위해선 Event를 통해 갱신해야 한다.
