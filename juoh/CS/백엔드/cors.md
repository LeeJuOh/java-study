# CORS

# CORS란?

---

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d75b0981-988d-4f92-8eed-de44d26f6e70/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d75b0981-988d-4f92-8eed-de44d26f6e70/Untitled.png)

- 위의 그림의 `CORS policy` 오류 메시지는 CORS 정책을 위반할 때 발생하게 된다.
- CORS는 Cross-Origin Resource Sharing의 약자입니다. 교차 출처 리소스 공유로 번역될 수 있는데, 브라우저에서 다른 출처의 리소스를 공유하는 방법

## URL 구조

- 다른 출처의 출처가 무엇인지 살펴봐야 하는데, 출처가 무엇인지 알기 위해서 먼저 URL의 구조를 살펴보아야 한다.
- URL 구조는 아래 그림과 같습니다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c9e1d3ca-b754-42d2-aa2e-90b56b9334e6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c9e1d3ca-b754-42d2-aa2e-90b56b9334e6/Untitled.png)

- 프로토콜의 HTTP는 80번, HTTPS는 443번 포트를 사용하는데, 80번과 443번 포트는 생략이 가능하다.

## 출처(Origin)란?

- 출처(Origin)란 URL 구조에서 살펴본 Protocal, Host, Port를 합친 것을 말한다.
- 브라우저 개발자 도구의 콘솔 창에 `location.origin`를 실행하면 출처를 확인할 수 있다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e299c221-f7ca-4c29-a25d-bfde6587a69a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e299c221-f7ca-4c29-a25d-bfde6587a69a/Untitled.png)

## 같은 출처 VS 다른 출처

- 현재 웹페이지의 주소가 `https://beomy.github.io/tech/`일 때 같은 출처인지 다른 출처인지 아래 테이블과 같은 결과를 얻을 수 있다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5b9ec315-da05-4067-b55c-93a5bcc183b8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5b9ec315-da05-4067-b55c-93a5bcc183b8/Untitled.png)

# 동일 출처 정책(Same-Origin Policy)이란?

- Postman으로 API를 테스트하거나, 다른 서버에서 API를 호출할 때는 멀쩡히 잘 동작하다가 브라우저에서 API를 호출할 때만 `CORS policy` 오류가 발생해서 당혹스러울 때가 있으셨을 수도 있다.
- 그 이유는 브라우저가 동일 출처 정책(Same-Origin Policy, SOP)를 지켜서 다른 출처의 리소스 접근을 금지하기 때문이다.
- 하지만 실제로 웹페이지는 상당히 자주 다른 출처의 리소스를 사용해야 합니다. 예를 들어 `beomy.github.io`라는 도메인 주소를 사용하는 웹페이지에서 `beomy-api.github.io`라는 API 서버로 데이터를 요청해서 화면을 그린다면 이 웹페이지는 동일 출처 정책을 위반한 것이 됩니다.

## 동일 출처 정책의 장점

- 동일 출처 정책을 지키면 외부 리소스를 가져오지 못해 불편하지만, 동일 출처 정책은 [XSS](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8C%85)나 [XSRF](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%9A%94%EC%B2%AD_%EC%9C%84%EC%A1%B0) 등의 보안 취약점을 노린 공격을 방어할 수 있습니다.
- 하지만 현실적으로는 외부 리소스를 참고하는 것은 필요하기 때문에 외부 리소스를 가져올 수 있는 방법이 존재해야 합니다.
    - 외부 리소스를 사용하기 위한 SOP의 예외 조항이 CORS이다.
    - SOP는 거의 모든 최신 브라우저에 구현되어 있기 때문에 한 출처의 웹사이트는 외국 출처의 리소스에 액세스할 수 없다.
    - CORS는 이를 가능하게 하는 메커니즘, 즉 보안을 완화하고 덜 제한적으로 만드는 방법이다.

# CORS 동작원리.

- CORS의 동작 방식은 2가지 방법이 있습니다.
    - Simple request
    - Preflighted Requests

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/348b4758-5c53-4feb-87f6-1ad2a55c2695/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/348b4758-5c53-4feb-87f6-1ad2a55c2695/Untitled.png)

## Simple request

- 단순 요청 방법은 서버에게 바로 요청을 보내는 방법이다.
    - 즉 Preflighted Requests를 trigger 하지 않는 요청

### Simple request 조건

- 서버로 전달하는 요청(request)이 아래의 3가지 조건을 만족해야 서버로 전달하는 요청이 단순 요청으로 동작한다.
    1. 요청 메서드(method)는 GET, HEAD, POST 중 하나여야 합니다.
    2. Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width를 제외한 헤더를 사용하면 안 됩니다.
    3. Content-Type 헤더는 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain` 중 하나를 사용해야 합니다.
- 첫 번째 조건은 어렵지 않은 조건이지만 2번, 3번 조건은 까다로운 조건이다.
- 2번 조건은 사용자 인증에 사용되는 `Authorization` 헤더도 포함되지 않아 까다로운 조건이며, 3번 조건은 많은 REST API들이 `Content-Type`으로 `application/json`을 사용하기 때문에 지켜지기 어려운 조건입니다.

### 자바스크립트에서 API를 요청할 때 브라우저와 서버의 동작 예제

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3ce9e2af-ec37-4310-bfd0-4acf9720283d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3ce9e2af-ec37-4310-bfd0-4acf9720283d/Untitled.png)

- `SesameStreet.com` 해당 페이지가 브라우저 탭에서 열린다. GET `CookieMonster.com/api/monsters` 대한 AJAX 요청(XMLHttpRequest 또는 Fetch API 사용)을 시작한다.
- 브라우저는 이것이 교차 출처 요청임을 인지하고 Origin 요청 헤더를 첨부합니다.

    ```jsx
    GET api/monsters HTTP/1.1
    Host: cookiemonster.com
    Origin: https://www.sesamestreet.com
    ```

- CORS로 구성된 서버는 Origin 헤더를 확인하고 Origin 허용되는 경우 Access-Control-Allow-Origin헤더를 다음 Origin 값으로 설정한다.

    ```jsx
    HTTP/1.1 200 OK
    Access-Control-Allow-Origin: https://www.sesamestreet.com  // or * ??
    ```

- 응답이 브라우저에 도달하 브라우저는 `Access-Control-Allow-Origin` 헤더의  값이 요청이 시작된 탭의 출처와 일치 하는지 확인한다.

## Preflight request

- 앞서 본 것처럼 `Authorization` 헤더를 추가하는 것조차 요청이 Preflight 된다.
- Preflight 요청은 서버에 예비 요청을 보내서 안전한지 판단한 후 본 요청을 보내는 방법이다.
    - 아래 그림은 Preflight 요청 동작을 나타내는 그림

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/94d1d723-1101-4a7f-8780-7a5db8931f6f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/94d1d723-1101-4a7f-8780-7a5db8931f6f/Untitled.png)

- `GET`, `POST`, `PUT`, `DELETE` 등의 메서드로 API를 요청했는데, 크롬 개발자 도구의 네트워크 탭에 `OPTIONS` 메서드로 요청이 보내지는 것을 보신 적 있으시다면 CORS를 경험했던 것.
- Preflight 요청은 실제 리소스를 요청하기 전에 `OPTIONS`라는 메서드를 통해 실제 요청을 전송할지 판단한다.
- `OPTIONS` 메서드로 서버에 예비 요청을 먼저 보내고, 서버는 이 예비 요청에 대한 응답으로 `Access-Control-Allow-Origin` 헤더를 포함한 응답을 브라우저에 보낸다.
- 브라우저는 단순 요청과 동일하게 `Access-Control-Allow-Origin` 헤더를 확인해서 CORS 동작을 수행할지 판단합니다.

### preflight request 예시

1. `SesameStreet.com`이 있는 브라우저 탭은 `POST` 메서드를 사용하여 JSON 페이로드로 `CookieMonster.com/api/monsters` 에 AJAX POST 요청을 한다.
    - 브라우저는 Content-Type을 확인하여 simple request가 아닌 POST를 요청인 것을 알고 있으므로 다음과 같은 세 가지 추가 매개 변수를 사용하여 Options 요청을 보낸다 .
    - 옵션은 세 가지 추가 매개 변수로 요청 전송하므로, 사전에 플라이트 할 수 있다.
        - Origin — this one we already know
        - Access-Control-Request-Method - HTTP method of the main (preflighted) request
        - Access-Control-Request-Headers - HTTP headers of the main (preflighted) request

        ```jsx
        OPTIONS /api/monsters HTTP/1.1
        Host: cookiemonster.com
        Origin: https://www.sesamestreet.com
        Access-Control-Request-Method: POST
        Access-Control-Request-Headers: Content-Type
        ```

2. 서버는 allowed origin, methods and headers를 응답합니다.

    ```jsx
    HTTP/1.1 200 OK
    Access-Control-Allow-Origin: https://www.sesamestreet.com
    Access-Control-Allow-Methods: POST, GET, OPTIONS
    Access-Control-Allow-Headers: Content-Type
    ```

3. origin이 허용되고 서버가 반환한 목록에 기본 요청의 HTTP 메서드와 헤더가 있는 경우 기본 요청을 전송할 수 있다.
    - 매번 preflight request 요청을 보내는 것은 성능 오버헤드가 될 수 있습니다. 이 문제는 `Access-Control-Max-Age` 응답 헤더를 사용하여 preflight request를 캐싱하여 완화할 수 있다.

## 다양한 cors 에러 해결 방법 참고

- [https://beomy.github.io/tech/browser/cors/](https://beomy.github.io/tech/browser/cors/)

# XSS(Cross Site Scripting)

xss는 주입식 공격이다. 공격자가 악의적인 스크립트를 신뢰할 수 있는 웹사이트에 삽입하는 방법의 공격이며 총 3가지 유형이 있다.

- Stored XSS: 보호되지 않고 검수되지 않은 사용자 입력으로 인한 취약점(데이터 베이스에 직접 저장되어 다른 사용자에게 표시됨)Reflected XSS: 웹 페이지에서 직접 사용되는 URL의 비보안에 의해 발생하는 취약점DOM based XSS: 웹페이지에서 직접 사용되는 URL의 비보안에 의해 발생한 취약점이라는 점에서 reflected XSS와 비슷하지만 DOM based XSS는 서버측으로 이동하지 않는다.

---

# CSRF(Cross site request forgery)

CSRF는 악의적인 웹사이트, 전자 메일, 블로그, 인스턴트 메시지 또는 프로그램으로 인해 사용자의 웹 브라우저가 사용자가 인증 된 다른 신뢰할 수 있는 사이트에서 원치 않는 작업을 수행 할 때 발생하는 공격 유형이다. 이 취약점은 브라우저가 세션 쿠키, IP주소 또는 각 요청과 유사한 인증 리소스를 자동으로 보내는 경우에 발생 할 수 있다.

[https://beomy.github.io/tech/browser/cors/](https://beomy.github.io/tech/browser/cors/)

[https://velog.io/@nayeon/CORS-개념과-간단한-XSS-CSRF-소개](https://velog.io/@nayeon/CORS-%EA%B0%9C%EB%85%90%EA%B3%BC-%EA%B0%84%EB%8B%A8%ED%95%9C-XSS-CSRF-%EC%86%8C%EA%B0%9C)

[https://dzone.com/articles/do-you-really-know-cors](https://dzone.com/articles/do-you-really-know-cors)
