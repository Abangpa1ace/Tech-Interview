# React
[JSX](#%EF%B8%8F-JSX)<br />
[Virtual Dom](#%EF%B8%8F-Virtual-Dom)<br />
[useEffect](#%EF%B8%8F-useEffect)<br />
[useState의 불변성](#%EF%B8%8F-useState의-불변성)<br />
[클래스 컴포넌트에서, 함수 컴포넌트로](#%EF%B8%8F-클래스-컴포넌트에서,-함수-컴포넌트로)<br />
[key 사용이유](#%EF%B8%8F-key-사용이유)<br />
[React의 이벤트](#%EF%B8%8F-React의-이벤트)<br />
[Babel, Webpack](#%EF%B8%8F-Babel,-Webpack)<br />
[Suspense 원리](#%EF%B8%8F-Suspense-원리)<br />
[Infinite Scroll 최적화](#%EF%B8%8F-Infinite-Scroll-최적화)<br />
<br />

**좋은글**<br />
- [React 렌더링](https://www.nextree.io/riaegteu-rendeoring-mic-coejeoghwa/)
- [동시성 렌더링과 Fiber](https://blog.mathpresso.com/react-deep-dive-fiber-88860f6edbd0)
<br />

## ✏️ JSX
JSX(Javascript eXtension) 은 자바스크립트의 확장문법이다. React는 HTML 요소를 표현하기 위해 JSX를 사용한다.<br />
마크업 언어처럼 보이나, 말 그대로 JSX는 Javascript 다. (Build 시, Babel에 의해 JS로 변환됨) 기본적으론 마크업 문법으로 작성할 수 있으며, 추가적인 문법이나 반드시 준수해야 할 주의점이 있다!<br />
- 반드시 하나의 부모 태그로 감싸준다. (태그로 특정할 필요가 없다면 빈 태그도 가능)
- 클래스명은 class가 아닌 className 으로 명명한다.
- 스타일 속성은 style={} 중괄호 안에 객체 형태로 포함시키며, 속성명은 카멜케이스로 적는다.
- 모든 태그는 반드시 클로징(" ... />") 되어야 한다. div와 같은 태그도 셀프 클로징 가능
- JSX의 {}(중괄호) 안에 Javascript 표현식을 사용할 수 있다.
- 주석은 // 도 가능하나, 기본적으론 {/* ~~ */} 형태이다.
<br />

## ✏️ Virtual Dom
SPA(Single Page Application) 은 DOM 조작이 많이 발생한다. 이러한 변경점마다 페이지 전체의 렌더 트리를 다시 그리는 것은 비효율적이다. (jQuery가 그랬다)<br />
Virtual DOM은 DOM을 추상화한 가벼운 복사본으로, 메모리엔 저장되나 실제 렌더되지 않는다. 이 가상DOM에 변경점을 반영하고, 현재 DOM과 대조하여 변경된 컴포넌트만 리렌더해주는 것이다. 이 과정을 **재조정(Reconciliation)** 이라고 칭한다.<br />

이전의 전체 리렌더하는 방식에 비해 브라우저의 연산량을 줄여 페이지 로드를 최적화할 수 있다.<br />
[https://velog.io/@sbinha/React에서-Virtual-DOM](https://velog.io/@sbinha/React%EC%97%90%EC%84%9C-Virtual-DOM)

<br />

## ✏️ useEffect
useEffect 훅은 함수 컴포넌트가 마운트 됐을 때, 언마운트 됐을 때, 업데이트 될 때 등, 생명주기와 비슷한 관점으로 특정 작업을 처리하기 위해 사용된다.(첫 번째 인자는 Effect 함수, 두 번째 인자는 Dependency Array)<br />
즉, 생명주기에 있다기 보다는 단순히 렌더링 후 사이드 이펙트를 실행하는 원리인 것이다.<br />
Effect 함수가 반환하는 함수를 clean-up 함수라고 하며 언마운트 시 실행된다.(컴포넌트 unmount ➡ cleanup 함수 실행 ➡ 컴포넌트 mount ➡ effect 실행)<br />

**useEffect 더 잘 쓰는 법**
- 단일 목적의 useEffect
- Custom Hook 화를 적극적으로
- 불필요한 Effect 로직은 조기에 return
- Effect에 관련된 모든 변수는 디펜던시에 추가
<br />

## ✏️ useState의 불변성
- 불변성을 위해 state는 객체(() => value)로 제공된다. 직접 변경해도 참조하는 자료주소는 불변하므로 리렌더링을 유발하지 않는다. (참조주소 변경이 객체 변경을 감지하는 확실한 방법이자 계산 리소스가 효율적)
- useState는 클로저로 구현되서 함수 선언시 값을 기억하고, state에 접근 가능한 setState 함수로 변경 가능하다. 불변성을 통해 이전 값을 참조할 수 있다.
- 결국, 상태변화 흐름을 알기 쉽고 예측못한 변경을 방지하기 위해 불변성을 보장해야한다.
<br />

## ✏️ 클래스 컴포넌트에서, 함수 컴포넌트로
- 함수는 클래스형보다 선언하기가 편하다. (Class 선언, Component 상속, constructor, render 함수, this 바인딩 등)
- 함수는 클래스형보다 메모리 자원을 덜 사용한다. (클래스는 인스턴스를 생성)
- 함수는 클래스형보다 빌드 후 파일크기가 더 작다. (클래스들의 메소드와 프로토타입을 가지고 있어 번들링 결과물이 커짐)
- 함수는 render() 함수가 필요 없어서 컴포넌트 마운트 속도가 빠르다.
- 함수는 immutable(불변성)을 유지할 수 있다. 클래스의 this는 변경 가능하나, 함수는 props(함수형 프로그래밍), state(클로저)의 불변성을 보장할 수 있다.
- 함수는 클래스형에 비해 내부로직 재사용이 용이하다.(hooks)
<br />

## ✏️ key 사용이유
반복적인 렌더링에서, 아이템 컴포넌트에 고유성을 부여해서 재조정(Reconciliation) 최적화
- key가 없으면 모든 아이템이 리렌더링 될 수 있음
- key를 인덱스 등 고유하지 않은 값을 사용하면, 의도치 않은 효과 발생가능(리스트 수정, 삭제, 중간삽입 등)
- 리스트 데이터가 변경 가능성이 없거나, 정적이거나, 내부에 상태값이 없다면 key가 반드시 필요하지 않음
<br />

## ✏️ React의 이벤트
- (on+이벤트명) 카멜 케이스로 프로퍼티를 명명하며, 여기에 핸들러 함수를 지정한다. 핸들러 함수는 렌더링 시점에 루트 DOM에 부착된다.(이벤트 위임, Delegation)
   - 메모리적 장점, SyntheticEvent 랩핑처리, 이벤트 간 우선순위 조정
- 이벤트 인자는 브라우저 Native Event를 랩핑한 SyntheticEvent 이다. 기본적인 처리추가(preventDefault)나, 일부 브라우저마다 다른 동작기능을 통일시켜준다.
   - SyntheticEvent는 Pooling(풀링)된다. 매 핸들러 실행마다 이벤트 객체를 초기화하면 비효율적이므로, 실행 시 EventPool에 SyntheticEvent 객체를 풀링하여 저장 및 재사용된다.
- 이벤트가 발생하면 버블링으로 루트에 도달한 뒤, 해당 타겟요소에 바인딩된 이벤트 함수를 dispatch한다.
<br />

## ✏️ Babel, Webpack
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

## ✏️ Suspense 원리
컴포넌트 렌더링에 필요한 대기 기능을 제공(fallback UI). 기존엔 Lazy Loading을 위한 실험적 기능이었으나, v18 정식출시 이후론 코드 스플리팅 목적뿐만 아니라 패칭 등 다양한 케이스를 대응한다. 
- 원리 : **Promise를 throw하여 상위에 비동기를 위임.** 감싸진 컴포넌트를 runPureTask 로직으로 구독하며, pending(throw Promise) / success(return result) / error(throw error)를 반환
<br />

## ✏️ Infinite Scroll 최적화
수백, 수천개의 컨텐츠를 표현한 경우 DOM이 그만큼 많아지면서 부하가 생기고, 스크롤/리사이징 시 버벅임 현상도 발생하게 된다.<br />
- Scroll Event(Throttle) 보다는 Intersection Observer API를 활용한다.
- 보이는 영역의 DOM만 표현한다. Scroll 값 혹은 컨테이너 order 등을 저장하고, 해당 인덱스 주변의 DOM 위주로 표현한다.
- 스크롤이 유지되어야 하므로, 이외는 가상 컴포넌트로 대체한다. (react-virtualized, react-window 등)
<br />

## ✏️ 단방향 데이터 바인딩
React는 단방향 데이터 바인딩 방식을 채택한다. 
**데이터 바인딩이란?** 두 데이터 혹은 정보의 소스를 일치시키는 기법. 즉, 화면의 데이터와 브라우저 메모리의 데이터를 일치시키는 기법이다.

**[양방향 바인딩]** <br />
<img src="https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/70e5fe9a-2bc4-40c4-93d2-608f9c37d8c2" width="400" />
Vue, Angular 는 양방향 바인딩을 채택한다. UI 및 JS 양쪽의 Watcher가 데이터를 동기화시켜주는 것이다. 이외 부가적인 로직이 불필요한 장점이 있지만, 데이터가 많아지면 Watcher도 증가하여 성능저하의 단점이 있다.
<br />

**[단방향 바인딩]** <br />
<img src="https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/024b6582-294d-410a-8f7c-9ccc4b9ea32b" width="400" />
React는 단방향 바인딩을 채택한다. 하나의 Watcher가 JS 데이터 갱신을 감지하여 UI를 업데이트한다. 반대로, 사용자가 상태값(혹은 JS)을 업데이트하기 위해선 Event를 통해 갱신해야 한다.
<br />
