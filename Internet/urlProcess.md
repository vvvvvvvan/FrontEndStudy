# 주소창에 URL을 입력하면 벌어지는 일

1. 입력
2. 브라우저는 URL을 해석한다.

 >  URL 구조: `scheme:[//[user:password@]host[:port]][/]path[?query][#fragment]`

3. URL이 문법에 맞으면 Punycode encoding을 URL의 HOST 부분에 적용한다.

4. HSTS(HTTP Strict Transport Security) 목록을 로드해서 확인한다.
 > HSTS 목록에 있으면 첫 요청을 HTTPS로 보내고, 아닌 경우 HTTP로 보낸다.

   > HTTP Strict Transport Security란?
    HTTP 대신 HTTPS만을 사용하여 통신해야 한다고 웹 사이트가 웹 브라우저에 알리는 보안기능

5. DNS를 조회한다.

  > * DNS(Domain Name Server)란?
    DNS는 도메인네임서버를 일컫는다.
    인터넷은 서버들을 유일하게 구분할 수 있는 IP주소를 기본체계로 이용하는데 숫자로 이루어진 조합이라 인간이 기억하기에는 무리가 따른다.
    따라서 DNS를 이용해 IP주소를 인간이 기억하기 편한 언어체계로 변환하는 작업이 필요한데 이 역할을 DNS가 하는 것이다.

6. ARP(Address Resolution Protocol)로 대상의 IP와 MAC Address를 알아낸다.

  > ARP(Address Resolution Protocol)란?
    IP주소를 MAC(Media Access Control)주소로 변환해주는 프로토콜

7. 대상과 TCP 통신을 통해 Socket을 연다.

8. HTTPS인 경우 TLS(Transport Layer Security) handshake가 추가 된다.

9. HTTP 프로토콜로 요청한다.