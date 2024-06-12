# HTML & CSS
[\<svg\>와 \<canvas\>](#%EF%B8%8F-\<svg\>와-\<canvas\>)<br />
[\<canvas\>의 CORS](#%EF%B8%8F-\<canvas\>의-CORS)<br />
[\<picture\> 태그와 반응형 이미지](#%EF%B8%8F-\<picture\>-태그와-반응형-이미지)<br />
<br />

## ✏️ \<svg\>와 \<canvas\>
- \<svg\> : XML 기반의 마크업 언어로, **확장 가능한 벡터(Vector) 그래픽**을 정의한다.<br />
해상도에 독립적, CSS 및 이벤트 핸들러 지원, 큰 화면에 적합, 텍스트에 유리 / 복잡한 DOM이나 웹게임엔 불리

- \<canvas\> : Javascript API와 함께 HTML 캔버스 Element로 **픽셀 기반 그래픽**을 제공하는 기법이다.<br />
해상도에 의존적, JS 위주의 기능적용, 픽셀이 많거나 복잡한 그래픽에 유리, 웹게임에 유리 / 텍스트 표현력은 불리
<br />

## ✏️ \<canvas\>의 CORS
### 보안과 오염된 캔버스
<img src="https://github.com/Abangpa1ace/Tech-Interview/assets/67219914/061a225c-cfeb-4458-855a-451f5b00e798" width="400" /><br />
캔버스의 비트맵 픽셀은 다른 호스트에서 불러온 이미지나 비디오 등 소스를 불러올 경우 보안문제를 야기할 수 있다.(CORS)
CORS 승인 없이 리소스를 그릴 경우 캔버스는 오염되고(tainted), 캔버스 컨텐츠를 읽으려고 시도하면 CORS 에러를 발생시킨다.
- 외부 컨텐츠를 \<img\>, \<svg\> 등으로 표현한 엘리먼트를 직접 검색하려는 경우
- 캔버스 컨텍스트에서 getImageData() 호출
- \<canvas\> 요소 자체에서 toBlob() , toDataURL() 또는 captureStream() 호출
<br />

### 해결방법
아래 다양한 방법으로 CORS를 해결하거나 우회할 수 있으며, 상황에 맞는 방식을 채택한다.
- img의 crossOrigin 속성을 'anonymous'로 설정
- AWS 어셋인 경우, S3 버킷 권한수정(Cache-Control) 혹은 Cloudfront CORS 설정
- React 등 라이브러리 경우, 이미지 Proxy 서버 설정
- 이미지 src를 base64 링크로 변환
<br />

## ✏️ \<picture\> 태그와 반응형 이미지
HTML의 \<picture\>, \<source\> 태그는 다양한 이미지 소스를 화면크기에 따라 최적화하고, 이미지 확장자를 적절히 적용할 때 사용된다.
- picture : 하나의 img 태그에 여러 개의 반응형 이미지를 제공하는, 0개 이상의 source 태그가 포함된 부모 요소
- source : picture 태그 내에서 반응형 이미지 소스를 설정하는 요소. srcset(리소스), media(반응형 조건), type(이미지 타입)

\* 이미지 최적화 : https://velog.io/@hustle-dev/%EC%9B%B9-%EC%84%B1%EB%8A%A5%EC%9D%84-%EC%9C%84%ED%95%9C-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EC%B5%9C%EC%A0%81%ED%99%94
  
