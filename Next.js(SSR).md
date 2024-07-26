# Next.js
[SSR](#%EF%B8%8F-SSR)<br />
[Image 태그](#%EF%B8%8F-Image-태그)<br />


## SSR
SSR(Server Side Rendering)은 페이지 HTML을 서버에서 Pre-rendering. 클라이언트는 이 페이지를 가져오고 추가로 필요한 JS파일을 다운로드한다.<br />
초기 페이지 로드가 빠르며, 서버패칭 보안 우수성이나 SEO 등 장점이 있다.
- SSR : 사용자가 페이지를 요청하는 시점마다 렌더링 (상품, 컨텐츠 등)
- SSG : 빌드 시에 페이지를 미리 생성. CDN으로 캐싱되어 이후 요청에 재사용됨 (소개, FAQ, 약관 등)
- ISR : 빌드 시에 페이지를 생성하고, 설정한 시간마다 리렌더링
<br />

## Image 태그
- lazy loading : viewpoint에 들어오기 전엔 미노출 or blur 이미지, 들어오면 정상 이미지
- 이미지 사이즈 최적화 : 적절한 이미지 사이즈(용량) 및 확장자(webp 등) -> 웹페이지 요청헤더 accept 속성으로 확인
- placeholder 제공 : 노출 전에 영역을 잡고 있으므로 CLS(Cumulative Layout Shift) 방지
- 이미지 캐싱 : minimumCacheTTL
