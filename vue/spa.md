# SPA

## Single-page application

- 기존의 전통적인 새로고침 방식 웹과는 달리 필요한 정적 파일을 다운로드 받고 이후 사용자와 상호작용하여 필요한 데이터만 서버로부터 동적으로 받아서 추가한다.

- 자바스크립트를 웹 브라우저에 사용하여 사용자 인터페이스(UI)를 표시하고 웹서버와 통신할 수 있다.

- 문서는 프로세스 중 다시 불러들이지 않지만, 위치 해시, HTML5 히스토리 API를 사용하여 애플리케이션 안에서 논리 문서의 인식 및 탐색을 제공한다.

- 장점

  - 트래픽의 총량을 줄인다.
  - 연속되는 페이지들 간의 사용자 경험의 간섭을 막아주고 애플리케이션이 데스크톱 애플리케이션 처럼 동작하도록 만든다.
  - 개발자가 작성해야하는 자바스크립트 코드의 양을 줄일 수 있다.

- 단점

  - 초기 구동 속도가 느리다
    -  초기 페이지에서 모든 리소스를 다운받지 않고, 리소스를 청크(Chunk)단위로 묶어서 해당 리소스에 대한 요청이 있을 때만 다운로드 받도록 함
  - 검색엔진 최적화(SEO) 문제
    - SEO에 노출되어야하는 전략적인 페이지를 선별해서 서버렌더링 기술을 적용한다.
  - 보안문제 (사용자 정보가 클라이언트 기반 쿠키에 저장되어 도용 될 가능성이 있다.)
    - 클라이언트에서 수행되는 핵심로직을 최소화한다.
  - IE8이하 지원
    - 최신의 자바스크립트 프레임워크들은 IE8을 지원대상에서 제외하고 있는 추세이다.




> 참고링크 
>
> [싱글 페이지 애플리케이션 - 위키백과](https://ko.wikipedia.org/wiki/%EC%8B%B1%EA%B8%80_%ED%8E%98%EC%9D%B4%EC%A7%80_%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98)
>
> [SPA 단점에 대한 단상](http://m.mkexdev.net/374)


