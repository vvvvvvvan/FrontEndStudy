# DOM (문서 객체 모델)

### DOM이란?

문서 객체 모델, 즉 DOM은 웹 페이지(HTML이나 XML 문서)의 콘텐츠 및 구조, 그리고 스타일 요소를 구조화 시켜 표현하여 프로그래밍 언어가 해당 문서에 접근하여 읽고 조작할 수 있도록 API를 제공하는 일종의 인터페이스이다. 즉 자바스크립트 같은 스크립팅 언어가 쉽게 웹 페이지에 접근하여 조작할 수 있게끔 연결시켜주는 역할을 한다.

### DOM(문서 객체 모델)은 어떻게 생성되고 어떻게 보여질까?

DOM은 웹 페이지, 즉 HTML 문서를 계층적 구조와 정보로 표현하며, 이를 제어할 수 있는 프로퍼티와 메서드를 제공하는 트리 자료구조이기도 하다. 따라서 HTML DOM, 혹은 HTML DOM Tree로 부르기도 한다.

>트리 자료구조는 노드들의 계층 구조로 이루어져 있다. 계층 구조로 이루어져 있기 때문에 부모-자식 관계, 형제관계를 표현하는 비선형 자료구조를 나타낸다.

트리 자료 구조로 구축이 되기 때문에, HTML 문서는 최종적으로 하나의 최상위 노드(root 노드)에서 시작해 자식 노드들을 가지며, 아래로만 뻗어나가는 구조로 만들어지게 된다.

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h2 style="color:blue">DOM 문서 객체 모델</h2>  
</body>
</html>
```

위 코드를 트리 구조로 표현하면 

document 노드가 최상위 노드가 되고, 밑으로 element 노드가 오며, 이어 text 노드와
attribute 노드가 오는 계층적인 구조이다.

### document node (문서 노드)

DOM Tree에서 최상위 루트 노드를 나타내며, document 객체를 가리킨다. HTML 문서 전체를 나타내는 노드이기도 하다. window 객체의 document 프로퍼티로 바인딩(연결)이 되어 있어 window.document, document로 참조해 사용할 수 있다. HTML 문서에 이 문서 노드는 오로지 1개만 존재한다.

### element node (요소 노드)

모든 HTML 요소(body,h2,div 등)는 이 요소 노드이다. 속성 노드를 가질 수 있는 유일한 노드로서,
부모 자식 관계를 가지게 되기 때문에 계층적 구조를 이룰 수 있게 된다.

### attribute node (속성 노드)

모든 HTML 요소의 속성은 이 속성 노드이다. 요소 노드에 대한 정보를 가지고 있다. 그렇게 때문에 부모 노드가 아닌 해당 노드와 연결(바인딩)이 되어 있다.

### text node (텍스트 노드)

HTML 문서의 모든 텍스트는 이 텍스트 노드라 해도 과언이 아니다. 텍스트 노드는 정보를 표현하며, 가장 
마지막에 위치하는 자식 노드이기 때문에 잎사귀를 닮았다 해 리프 노드라고 불리기도 한다.

이 4가지 노드들이 존재함으로써 스크립팅 언어가 웹페이지에 접근하고 조작할 수 있게 된다.
특히 데이터 검색하기가 빠른 트리 구조로 이루어져 있기 때문에 이 접근하고 조작하여 업데이트를 하는 속도는 빠른 편이다.

### JAVASCRIPT와 DOM, 다른 개념인 이유는?

거의 한 몸처럼 사용하게 되니, 다음과 같은 오해가 생기곤 한다. "자바스크립트 안에 DOM이 있는 거 아니냐", 혹은 "DOM은 자바스크립트로만 다룰 수 있는 거 아니냐" 같은 심심한 오해가 생기게 되곤 한다.

그러나 자바스크립트와 DOM은 엄밀히 다른 개념이며, 꼭 자바스크립트로만 DOM을 다룰 수 있는 것도 아니다.

DOM은 자바스크립트 없이 DOM 인터페이스 구현만으로도 DOM을 조작할 수 있기 때문이다. DOM은 앞서 프로그래밍 언어가 해당 문서에 접근하여 읽고 조작할 수 있도록 API를 제공하는 일종의 인터페이스라고 설명했다. 조금 더 부연 설명을 하자면 DOM은 어떤 프로그래밍 언어에 의존하지 않는 독립적인 인터페이스 라는 것이다.

이렇게 때문에 DOM은 꼭 자바스크립트로만 구현 되는 것도 아니며 다른 프로그래밍 언어인 자바로도 구현할 수 있으며, C#으로도 구현할 수 있다.

### DOM이 자바스크립트라는 오해의 원인

현재 웹 브라우저에서 DOM을 조작하는 언어가 자바스크립트 뿐이기 때문이다.
브라우저는 과거 HTML 문서로만 이루어진 웹 페이지를 출력하기만 했고, 그것만으로도 충분했다.
그러나 점점 시간이 흐르며 동적인 기능을 요구하기 시작했고, HTML 문서 만으로는 이 기능을 제공하기 불가능 해졌다.

그래서 웹 브라우저 내부에 이 동적인 기능을 지원해줄 프로그래밍 언어를 넣기로 했는데, 현재의 자바스크립트 초석이 되는 Mocha(모카)이다. 웹 브라우저는 이 때 이후 내장 프로그래밍 언어로 쭉 자바스크립트를 고수해오고 있다. 그렇기 때문에 다른 언어로도 DOM을 조작할 수 있는 것은 분명하나, 브라우저를 꽉 잡고 있는 자바스크립트의 아성을 이기지 못하는 듯 하다.

### DOM의 정적 생성과 동적 생성

지금 현재 브라우저에 내장되어 있는 언어는 자바스크립트이고, 자바스크립트는 가장 간편하고 빠르게 DOM으로 구조화된 웹 문서에 접근하여 노드(웹 컨텐츠를 이루는 기본 요소)들을 조작할 수 있다.

엄밀히 말하자면, 자바스크립트를 이용해 HTML 문서에 없는 노드를 만들어 이어 붙여 웹 페이지에 렌더링되게 만드는 모든 과정이 동적으로 구현하는 것이라 볼 수 있다. 또는 자바스크립트를 이용해 있던 노드에 없는 노드를 만들어 이어 붙이는 것도 동적으로 구현한다고 불 수 있다.

정적으로 생성되는 과정은 오로지 이미 HTML 파일에 적혀 있는 코드를 위에서부터 아래로 읽어내려가며 생성하는 과정만을 뜻한다. 즉 HTML 문서에 직접 태그로 작성하는 것만을 정적으로 생성한다고 보기 때문에, 이런 부분에서 차이가 날 수 밖에 없다.

### DOM의 데이터타입

객체의 구성 요소부터 살펴보면

* 프로퍼티(property) : DOM 객체의 멤버 변수이다. HTML 태그의 속성을 반영한다.
* 메소드(method) : DOM 객체의 멤버 함수이다. HTML 태그를 제어한다.
* 컬렉션(collection) : 정보를 집합적으로 표현하는 일종의 배열이다. 예를 들어 childern 컬렉션은 DOM 객체의 모든 자식 DOM 객체에 대한 주소를 가진다.
* 이벤트 리스너(event listener) : HTML 태그에 작성된 이벤트 리스너(onclick, onchange 등)들을 그대로 가진다.
* 스타일(style) : 이 프로퍼티를 통해 HTML 태그에 적용된 CSS 스타일 시트에 접근 가능하다.

### JavaScript DOM 접근하는 방법

자바스크립트로 DOM에 접근하는 방법은, DOM의 인터페이스를 이용하여 접근할 수 있다.
기본적으로 브라우저 내부에 내장된 프로그래밍 언어(즉 자바스크립트)가 DOM의 API 중 자주 쓰는 메소드와 프로퍼티가 있는데,

* document.querySelectorAll(selectors)
* document.getElementById(id)
* document.getElementByTagName(name)
* document.createElement(name)
* node.append(node)
* node.appendChild(node)
* node.remove(node)
* node.removeChild(node)
* element.innerHTML
* node.textContent
* element.setAttribute(name, value)
* element.getAttribute(name)
* element.addEventListener(type, listener)

등이 있다.

### 주요 노드 프로퍼티

DOM 노드 클래스

DOM 노드는 종류에 따라 각각 다른 프로퍼티를 지원한다. 태그 <a>에 대응하는 요소 노드엔 링크 관련된 프로퍼티를, <input>에 대응하는 요소 노드엔 입력 관련 프로퍼티를 제공한다. 
그런데 모든 DOM 노드는 공통 조상으로부터 만들어지기 때문에 노드 종류는 다르지만, 모든 노드는 공통된 프로퍼티와 메서드를 지원한다.

DOM 노드는 종류에 따라 대응하는 내장 클래스가 다르다.

계층 구조 꼭대기엔 EventTarget이 있는데, Node는 EventTarget을, 다른 DOM 노드들은 Node 클래스를 상속받는다.

* EventTarget - 루트에 있는 '추상(abstract) 클래스로' 이 클래스에 대응하는 객체는 실제로 만들어지지 않는다. EventTarget이 모든 DOM 노드의 베이스에 있기 때문에 DOM 노드에서 '이벤트'를 사용할 수 있다.

* Node - 역시 '추상' 클래스로, DOM 노드의 베이스 역할을 한다. getter 역할을 하는 parentNode.
nextsibling, childNodes 등의 주요 트리 탐색 기능을 제공한다. Node 클래스의 객체는 절대 생성되지 않는다.
하지만 이 클래스를 상속받는 클래스는 여럿 있다. 텍스트 노드를 위한 Text 클래스와 요소 노드를 위한 Element 클래스, 주석 노드를 위한 Comment 클래스는 Node 클래스를 상속받는다.

* Element - DOM 요소를 위한 베이스 클래스이다. nextElementSibling, children이나 getElementByTagName, QuerySelector 같이 요소 전용 탐색을 도와주는 프로퍼티나 메서드가 이를 기반으로 한다. 브라우저는 HTML 뿐만 아니라 XML, SVG도 지원하는데 Element 클래스는 이와 관련된 SVGElement, XMLElement, HTMLElement 클래스의 베이스 역할을 한다.

* HTMLElement - HTMl 요소 노드의 베이스 역할을 하는 클래스이다. 아래 나열한 클래스들은 실제 HTMl 요소에 대응하고 HTMLElement를 상속받는다.

이렇게 특정 노드에서 사용할 수 있는 프로퍼티와 메서드는 상속을 기반으로 결정된다.

<input> 요소에 대응하는 DOM 객체를 예로 들어보면, 이 객체는 HTMLInputElement 클래스를 기반으로 만들어진다.

객체엔 아래에 나열한 클래스에서 상속받은 프로퍼티와 메서드가 있다.

* HTMLInputElement - 입력 관련 프로퍼티를 제공하는 클래스
* HTMLElement - HTML 요소 메서드와 getter, setter를 제공하는 클래스
* Element - 요소 노드 메서드를 제공하는 클래스
* Node - 공통 DOM 노드 프로퍼티를 제공하는 클래스
* EventTarget - 이벤트 관련 기능을 제공하는 클래스
* Object - hasOwnProperty 같이 '일반 객체' 메서드를 제공하는 클래스

정리하자만

주요 노드 프로퍼티는 다음과 같다.

nodeType
> 요소 타입을 알고 싶을 때 사용한다. 요소 노들면 1을, 텍스트 노드라면 3을 반환한다. 두 타입 외에도 각 노드 타입엔 대응하는 상숫값이 있다. 읽기 전용이다.

nodeName/tagName

> 요소 노드의 태그 이름을 알아낼 때 사용한다. XML 모드일 때를 제외하고 태그 이름은 항상 대문자로 변환한다.
요소 노드가 아닌 nodeName을 사용하면 된다.

innerHTML
> 요소 안의 HTML을 알아낼 수 있다. 이 프로퍼티를 사용하면 요소 안의 HTML을 수정할 수도 있다.

outerHTML
> 요소의 전체 HTML을 알아낼 수 있다.

nodeValue/data
> 요소가 아닌 노드(텍스트,주석 노드 등)의 내용을 읽을 때 쓰인다. 두 프로퍼티는 거의 동일하게 동작한다.
주로 data를 많이 사용하는 편이며 내용을 수정할 때도 이 프로퍼티를 쓸 수 있다.

textContent
> HTML에서 모든 태그를 제외한 텍스트만 읽을 때 사용한다. 할당 연산을 통해 무언가를 쓸 수 있는데 이 때 태그를 포함한 모든 특수문자는 문자열로 처리된다. 사용자가 입력한 문자를 안전한 방법으로 처리하기 때문에 원치 않는 HTMl이 사이트에 삽입되는 것을 예방할 수 있다.


hidden
> true로 설정하면 CSS에서 display; none을 설정한 것과 동일하게 동작한다.

### 문서 수정하기

* 노드 생성 메서드:
    * document.createElement(tag) - 태그 이름을 사용해 새로운 요소를 만듦
    * document.createTextNode(value) - 텍스트 노드를 만듦(잘 쓰이지 않음)
    * elem.cloneNode(deep) - 요소를 복제함
        deep==true 일 경우 모든 자손 요소도 복제됨
* 노드 삽입, 삭제 메서드:
    * node.append(노드나 문자열) - node 끝에 노드를 삽입
    * node.prepend(노드나 문자열) - node 맨 앞에 노드를 삽입
    * node.before(노드나 문자열) - node 이전에 노드를 삽입
    * node.after(노드나 문자열) - node 다음에 노드를 삽입
    * node.replaceWith(노드나 문자열) - node를 대체
    * node.remove() - node를 제거


문자열을 삽입, 삭제할 땐 문자열을 '그대로' 넣으면 된다.

