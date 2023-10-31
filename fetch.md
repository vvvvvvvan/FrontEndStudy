자바스크립트를 사용하면 필요할 때 서버에 네트워크 요청을 보내고, 새로운 정보를 받아오는 일을 할 수 있다.

페이지 새로 고침 없이 가능한데,
AJAX(Asynchronous JavaScript And XML)를 사용한다.

이는 서버에서 추가 정보를 비동기적으로 가져올 수 있게 해주는 포괄적인 기술을 나타내는 용어로, 만들어진지 오래되었다.

그래서 XML이 포함된 이유가 바로 이 때문

AJAX 말고도 요청하는 방법은 여러가지가 있다.

그 중 하나가 fetch() 메소드이다.

```js
let promise = fetch(url, [options])
```
* url - 접근하고자 하는 url
* options - 선택 매개변수, method나 header 등을 지정할 수 있음

options에 아무것도 넘기지 않으면 요청은 GET 메서드로 진행되어 url로 부터 콘텐츠가 다운로드 된다.

fetch()를 호출하면 브라우저는 네트워크 요청을 보내고 프라미스가 반환된다.
반환되는 프라미스는 fetch()를 호출하는 코드에서 사용된다.

응답은 대개 두 단계를 거쳐 진행되며

먼저, 서버에서 응답 헤더를 받자마자 fetch 호출 시 반환받은 promise가 내장 클래스 Response의 인스턴스와 함께 이행 상태가 된다.

이 단계는 아직 본문(body)이 도착하기 전이지만, 개발자는 응답 헤더를 보고 요청이 성공적으로 처리되었는지 아닌지를 확인할 수 있다.


일반적인 fetch 요청은 두 개의 await 호출로 구성된다.

```js
let response = await fetch(url, options); // 응답 헤더와 함께 이행됨
let result = await response.json(); // json 본문을 읽음
```

물론 await 없이도 요청을 보낼 수 있다.
```js
fetch(url, options)
    .then(response => response.json())
    .then(result => /*결과 처리*/)
```
응답 객체의 프로퍼티는 다음과 같다.

response.status – 응답의 HTTP 코드
response.ok – 응답 상태가 200과 299 사이에 있는 경우 true
response.headers – 맵과 유사한 형태의 HTTP 헤더
응답 본문을 얻으려면 다음과 같은 메서드를 사용하면 된다.

response.text() – 응답을 텍스트 형태로 반환함
response.json() – 응답을 파싱해 JSON 객체로 변경함
response.formData() – 응답을 FormData 객체 형태로 반환(form/multipart 인코딩에 대한 내용은 다음 챕터에서 다룸)
response.blob() – 응답을 Blob(타입이 있는 바이너리 데이터) 형태로 반환
response.arrayBuffer() – 응답을 ArrayBuffer(바이너리 데이터를 로우 레벨로 표현한 것) 형태로 반환


* fetch를 사용해 GITHUB에서 사용자 정보 가져오기

GitHub 사용자 이름이 담긴 배열을 인자로 받는 비동기 함수 getUsers(names)를 만든 후, GitHub에서 fetch한 사용자 정보들이 담긴 배열을 반환하는 함수를 만들어 보자.

사용자명에 해당하는 사용자 정보를 가져오려면 GitHub API https://api.github.com/users/사용자명에 요청을 보내면 된다.

조건
1. 사용자당 fetch 요청은 한 번만 수행해야 한다.
2. 데이터가 최대한 일찍 도착할 수 있도록 각 요청은 다른 요청의 결과를 기다려서는 안된다.
3. 요청을 실패하거나 존재하지 않는 사용자에 대한 요청을 보냈다면 null을 리턴하고 배열 요소를 담아야 한다.

```js
async function getUsers(names) {
  let jobs = [];

  for(let name of names) {
    let job = fetch(`https://api.github.com/users/${name}`).then(
      successResponse => {
        if (successResponse.status != 200) {
          return null;
        } else {
          return successResponse.json();
        }
      },
      failResponse => {
        return null;
      }
    );
    jobs.push(job);
  }

  let results = await Promise.all(jobs);

  return results;
}
```
