# CORS

>fetch로 요청을 보내게 될 사이트가 현재 접속 사이트와 다르다면 요청이 실패할 수 있다.
왜 요청이 실패하는지를 알기 위해선 도메인.프로토콜.포트 세 가지에 의해 결정되는 오리진(origin)이라는 핵심 개념을 알아야 한다.

도메인이나 서브 도메인, 프로토콜, 포트가 다른 곳에 요청을 보내는 것을 Cross-Origin-Request(크로스 오리진 요청) 이라고 한다.

크로스 오리진 요청을 보내려면 '리모트 오리진에서 전송받은 특별한 헤더'가 필요하다.

이러한 정책을 'CORS(Cross-Origin-Resource Sharing)', 크로스 오리진 리소스 공유라고 부른다.

## #왜 CORS가 필요한가

악의를 가진 해커로부터 인터넷을 보호하기 위해 만들어졌다고 보면 된다.

브라우저 관점에선 크로스 오리진 요청은 안전한 요청과 그렇지 않은 요청으로 나뉜다.

안전한 요청은 다음 조건을 모두 충족하는 요청이다.

* 메서드: GET이나 POST 혹은 HEAD
* 헤더:
    * Accept
    * Accept-Language
    * Content-Language
    * 값이 application/x-www-form-urlencoded나 multipart/form-data, text/plain인 Content-Type

안전한 요청은 아주 오래전 부터 <form>이나 <script> 태그를 사용해도 가능했던 요청인 반면,
 안전하지 않은 요청은 브라우저에선 보낼 수 없었던 요청이라는 점이 두 요청의 근본적인 차이이다.

실무 관점에서 두 요청의 차이는 안전한 요청은 Origin 헤더와 함께 바로 요청이 전송되는 반면, 
안전하지 않은 요청은 브라우저에서 본 요청이 이루어지기 전에 preflight 요청이라 불리는 사전 요청을 보내 퍼미션 여부를 물어본다는 점이다.

안전한 요청은 다음과 같은 절차를 따른다.

→ 오리진 정보가 담긴 Origin 헤더와 함께 브라우저가 요청을 보낸다.
← 자격 증명이 없는 요청의 경우(기본), 서버는 아래와 같은 응답을 보낸다.
Origin 값과 동일하거나 *인 Access-Control-Allow-Origin
← 자격 증명이 있는 요청의 경우 서버는 아래와 같은 응답을 보낸다.
Origin 값과 동일한 Access-Control-Allow-Origin
값이 true인 Access-Control-Allow-Credentials

자바스크립트를 사용해 Cache-Control이나 Content-Language, Content-Type, Expires, Last-Modified, Pragma를 제외한 응답 헤더에 접근하려면 응답 헤더의 Access-Control-Expose-Headers에 접근을 허용하는 헤더가 명시돼 있어야 한다.

안전하지 않은 요청의 절차는 다음과 같다.
사전요청인 'preflight' 요청이 본 요청 전에 전송된다.

→ 브라우저는 동일한 URL에 OPTIONS 메서드를 사용한 preflight 요청을 보내게 되는데, 이때 헤더엔 다음과 같은 정보가 들어간다.

    *Access-Control-Request-Method – 본 요청의 메서드 정보가 담김
    *Access-Control-Request-Headers – 본 요청의 헤더 정보가 담김
    *← 서버는 상태 코드 200과 아래와 같은 헤더를 담은 응답을 전송한다.
    *Access-Control-Allow-Methods – 허용되는 메서드 목록이 담김
    *Access-Control-Allow-Headers – 허용되는 헤더 목록이 담김
    *Access-Control-Max-Age – 몇 초간 preflight 요청 없이 크로스 오리진 요청을 바로 보낼지에 대한 정보가 담김

이후엔 본 요청이 전송되고, 절차는 ‘안전한’ 요청과 동일하다.

### 왜 오리진이 필요하나

1. Referer에 더 많은 정보가 있을 수 있는데 왜 Origin을 따로 두었는가
2. 헤더에 Referer나 Origin이 없을 수도 있을 텐데 이런 경우엔 어떤 일이 발생하나


> 간혹 Referer가 없는 경우가 있기 때문에 Origin이 필요하다. HTTPS 프로토콜을 사용해 HTTP 페이지를 요청하는 경우같이 보안 수준이 높은 방법을 사용해 보안 수준이 낮은 페이지에 접근할 땐 Referer가 없다.

이 외에도 콘텐츠 보안 정책이 적용돼 Referer 정보가 누락되는 경우도 있다.

fetch에는 Referer 전송을 막는 옵션이 있습니다. 동일한 사이트 내에서 Referer 수정을 허용하는 옵션도 있다.

명세서에 따르면, Referer는 HTTP 헤더에서 선택 사항이다.

이렇게 Referer는 신뢰할 수 없는 정보를 담고 있을 확률이 높기 때문에 명세서에 Origin이 추가되었다. 크로스 오리진 요청 시 브라우저는 Origin이 제대로 전송되는 것을 보장한다.


