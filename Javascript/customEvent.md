이벤트 디스패치

자바스크립트를 사용하면 핸들러를 할당할 수 있을 뿐만 아니라 이벤트를 직접 만들 수도 있다.

새로운 커스텀 이벤트뿐만 아니라 목적에 따라 click, mousedown 같은 내장 이벤트를 직접 만들수도 있습니다.

이렇게 만든 내장 이벤트들은 테스팅을 자동화할 때 유용하다.

Event의 생성자

내장 이벤트 클래스는 DOM 요소 클래스 같이 계층 구조를 형성한다.

내장 이벤트 클래스 계층의 꼭대기엔 Event 클래스가 있다.

Event 객체는 다음과 같이 생성할 수 있다.

let event = new Event(type[, options]);

type - 이벤트 타입을 나타내는 문자열로 "click" 같은 내장 이벤트, "my-event" 같은 커스텀 이벤트가 올 수도 있다.

options - 두 개의 선택 프로퍼티가 있는 객체가 온다.

bubbles: true/false-true인 경우 이벤트가 버블링 된다.
cancelable: true/false - true인 경우 브라우저 '기본 동작'이 실행되지 않는다. 자세한 내용은 커스텀 이벤트 섹션에서

아무런 값도 지정하지 않으면 두 프로퍼티는 기본적으로 { bubbles: false, cancelable: false } 처럼 false가 된다.

dispatchEvent

이벤트 객체를 생성한 다음엔 elem.dispatchEvent(event)를 호출해 요소에 있는 이벤트를 반드시 '실행'시켜
이렇게 이벤트를 실행시켜줘야 핸들러가 일반 브라우저 이벤트처럼 이벤트에 반응할 수 있다.

bubbles 플래그를 true로 해서 이벤트로 만든 경우 이벤트는 제대로 버블링 된다.

예시를 살펴보자

자바스크립트를 사용해 click 이벤트를 만들고 실행시켜 보았다.

커스텀 이벤트 버블링

"hello"라는 이름을 가진 이벤트를 만들고 버블링 시켜서 document에서 이벤트를 처리할 수 있게 해보자.

<h1 id="elem">Hello from the script!</h1>

<script>
  // 버블링이 일어나면서 document에서 이벤트가 처리됨
  document.addEventListener("hello", function(event) { // (1)
    alert("Hello from " + event.target.tagName); // Hello from H1
  });

  // 이벤트(hello)를 만들고 elem에서 이벤트 디스패치
  let event = new Event("hello", {bubbles: true}); // (2)
  elem.dispatchEvent(event);

  // document에 할당된 핸들러가 동작하고 메시지가 얼럿창에 출력됩니다.

</script>

위 예시에서 주의해서 볼 점은 다음과 같습니다.

on<event>는 내장 이벤트에만 해당하는 문법이기 때문에 document.onhello라고 하면 원하는 대로 동작하지 않는다.

커스텀 이벤트는 반드시 addEventListener를 사용해 핸들링해야한다.

bubbles:true를 명시적으로 설정하지 않으면 이벤트가 버블링 되지 않는다.

UI 이벤트 주의점

명세서의 UI 이벤트 섹션엔 다양한 UI 이벤트 클래스가 명시되어 있다.

그중 일부를 추리면 다음과 같다.

UIEvent
FocusEvent
MouseEvent
WheelEvent
KeyboardEvent

그런데 이 이벤트들은 new Event로 만들면 안되고, 반드시 관련 내장 클래스를 사용해야 한다.

마우스 클릭 이벤트라면 new MouseEvent("click")를 사용해야한다.

이렇게 제대로 된 생성자를 사용해야만 하는 해당 이벤트 전용 표준 프로퍼티를 명시할 수 있다.

new MouseEvent("click")를 사용해 마우스 이벤트의 clientX,clientY 프로퍼티를 설정해 본다.

let event = new MouseEvent("click", {
  bubbles: true,
  cancelable: true,
  clientX: 100,
  clientY: 100
});

alert(event.clientX); // 100


// 작동 안됨 !!!!!!!!!!!!!!!!!!!!!!
let event = new Event("click", {
  bubbles: true, // Event 생성자에선
  cancelable: true, // bubbles와 cancelable 프로퍼티만 동작합니다.
  clientX: 100,
  clientY: 100
});

alert(event.clientX); // undefined, 알 수 없는 프로퍼티이기 때문에 무시됩니다.


커스텀 이벤트 만들기

지금까진 new Event로 커스텀 이벤트를 만들었다.
하지만 제대로 된 커스텀 이벤트를 만들려면 new CustonEvent를 사용해야 한다.

CustonEvent는 Event와 거의 유사하지만 한 가지 다른 점이 있다.

CustomEvent의 두 번째 인수엔 객체가 들어갈 수 있는데, 개발자는 이 객체에 detail이라는 프로퍼티를 추가해 커스텀 이벤트 관련 정보를
명시하고, 정보를 이벤트에 전달 할 수 있다.

detail 프로퍼티엔 어떤 데이터도 들어갈 수 있다.

사실 new Event로 일반 이벤트를 생성한 다음 추가 정보가 담긴 프로퍼티를 이벤트 객체에 추가해주면 되기 때문에 detail 프로퍼티 없이도 
충분히 이벤트에 원하는 정보를 추가할 수 있긴 하다.

그런데도 detail 이라는 특별한 프로퍼티를 사용하는 이유는 다른 이벤트 프로퍼티와 충돌을 피하기 위해서이다.

이 외에도 new CustomEvent를 사용하면 코드 자체만으로 '커스텀 이벤트'라고 설명해주는 효과가 있다.

동기적 처리 (이벤트 안 이벤트)

이벤트는 대게 큐에서 처리된다.

따라서 브라우저가 onclick 이벤트를 처리하고 있는데 마우스를 움직여서 새로운 이벤트를 발생시키면 이 이벤트를 상응하는
mousemove 핸들러는 onclick 이벤트 처리가 끝난 후에 호출된다.

그런데 이벤트 안 dispatchEvent 처럼 이벤트 안에 다른 이벤트가 있는 경우엔 위와 같은 규칙이 적용되지 않는다.

이벤트 안에 있는 이벤트 즉시 처리된다.

그런데 이벤트 안 dispatchEvent 처럼 이벤트 안에 다른 이벤트가 있는 경우엔 위와 같은 규칙이 적용되지 않는다.

이벤트 안에 있는 이벤트는 즉시 처리된다. 새로운 이벤트 핸들러가 호출되고 난 후에 현재 이벤트 핸들링이 재개된다.

예시를 살펴보자

<button id="menu">메뉴(클릭해주세요)</button>

<script>
  menu.onclick = function() {
    alert(1);

    menu.dispatchEvent(new CustomEvent("menu-open", {
      bubbles: true
    }));

    alert(2);
  };

  // 1과 2 사이에 트리거됩니다
  document.addEventListener('menu-open', () => alert('중첩 이벤트'));
</script>

얼럿창에 '1'. '중첩 이벤트', '2'가 차례대로 출력되는 것을 확인할 수 있다.


이 예시에서 주목해야 할 것은 중첩 이벤트 menu-open이 document에 할당된 핸들러에서 처리된다는 점이다.

중첩 이벤트의 전파와 핸들링이 외부 코드(onclick)의 처리가 다시 시작되기 전에 끝났다.

이런 일은 중첩 이벤트가 dispatchEvent일 때 뿐만 아니라 이벤트 핸들러 안에서 다른 이벤트를 트리거 하는 메서드를 호출할 때 발생한다.

즉, 이벤트 안 이벤트는 동기적으로 처리된다.

비동기적 처리하기

그런데 때에 따라 중첩 이벤트가 동기적으로 처리되는걸 원치 않는 경우도 있기 마련이다.


<button id="menu">Menu (click me)</button>

<script>
  menu.onclick = function() {
    alert(1);

    setTimeout(() => menu.dispatchEvent(new CustomEvent("menu-open", {
      bubbles: true
    })));

    alert(2);
  };

  document.addEventListener('menu-open', () => alert('중첩 이벤트'));
</script>

