# 속성과 프로퍼티

브라우저는 웹페이지를 만나면 HTML을 읽어(파상(parsing)) DOM 객체를 생성한다. 요소 노드(element node)에서 대부분의 표준 HTML 속성(attribute)은 DOM 객체의 프로퍼티(property)가 된다.

태그 <body id="page">가 있을 때, DOM 객체에서 body.id="page"를 사용할 수 있는 것 같이 말이다.

그런데 속성.프로퍼티가 항상 일대일로 매핑되지는 않는다. 이번 챕터에선 속성과 프로퍼티를 어떻게 다룰 수 있는지, 두 가지가 언제 일대일로 매핑되는지, 언제는 매핑되지 않는지에 주의하면서 두 개념을 알아보자.

### DOM 프로퍼티
앞서 내장 DOM 프로퍼티에 대해 살펴본 바 있다. DOM 프로퍼티의 종류는 엄청나게 많다. 하지만 이런 내장 프로퍼티 만으로 충분하지 않은 경우, 자신만의 프로퍼티를 만들 수도 있다.

DOM 노드는 자바스크립트 객체이다. 객체를 바꿔보자.

```JavaScript
document.body.myData = {
    name: 'Caesar',
    title: 'Imperator'
};

alert(document.body.myData.title); // Imperator

//메서드를 하나 추가해보자

document.body.sayTagName = function () {
    alert(this.tagName);
};
document.body.sayTagName(); // BODY (sayTagName의 'this'엔 document.body가 저장된다.)
```

Element.prototype 같은 내장 프로토타입을 수정해 모든 요소 노드에서 이 메서드를 사용하게 할 수도 있다.

```JavaScript
Element.prototype.sayHi = function() {
  alert(`Hello, I'm ${this.tagName}`);
};

document.documentElement.sayHi(); // Hello, I'm HTML
document.body.sayHi(); // Hello, I'm BODY
```

HTMl 속성

HTML에서 태그는 복수의 속성을 가질 수 있다. 브라우저는 HTMNL을 파싱해 DOM 객체를 만들 때 HTML 표준 속성을 인식하고, 이 표준 속성을 사용해 DOM 프로퍼티를 만든다.

따라서 요소가 id 같은 표준 속성으로만 구성되어 있다면, 이에 해당하는 프로퍼티가 자연스레 만들어진다. 하지만 표준이 아닌 속성일 때는 상황이 달라진다.

```HTMl
<body id="test" something="non-standard">
  <script>
    alert(document.body.id); // test
    // 비표준 속성은 프로퍼티로 전환되지 않습니다.
    alert(document.body.something); // undefined
  </script>
</body>
```

한 요소에선 표준인 속성이 다른 요소에선 표준이 아닐 수 있다는 점도 주의해야 한다. "type"은 <input> 요소에선
표준이지만 <body>에선 표준이 아니다. 요소에 어떤 표준 속성이 있는지 알아보려면 해당 요소의 명세서의 정보를 찾을 수 있다.

```HTMl
<body id="body" type="...">
  <input id="input" type="text">
  <script>
    alert(input.type); // text
    alert(body.type); // type은 body의 표준 속성이 아니므로 DOM 프로퍼티가 생성되지 않아 undefined가 출력됩니다.
  </script>
</body>
```

이런 비표준 속성일때 접근하는 방법은


elem.hasAttribute(name) – 속성 존재 여부 확인
elem.getAttribute(name) – 속성값을 가져옴
elem.setAttribute(name, value) – 속성값을 변경함
elem.removeAttribute(name) – 속성값을 지움

으로 접근한다.
