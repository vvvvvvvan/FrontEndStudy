window onload()를 사용하는 이유

HTML 실행 위치에 따른 오작동

HTML 문서는 객체 태그들을 위에서부터 아래로 차례차례 읽어 들인다. 그런데 이러한 특성으로 인해 
가끔 자바스크립트의 작성 위치에 따라 오작동을 일으키기도 한다.

예를 들어 아래 코드와 같이 <script> 태그의 자바스크립트에서 태그의 id가 'name'인 엘리멘트 요소를 가져와 색깔을 파란색으로 
바꿔주는데, 가져오려는 <p id="name">hello</p> 엘리먼트가 <script> 태그 밑에 위치해 있을 경우 문제가 일어나게 된다.

 <html>
    <body>
        <script>
            let a = document.getElementById('name');
            a.style.color = "blue"
        </script>

        <p id="name">hello</p>
    </body>
</html>

이러한 까닭은 HTML은 실행 이전에 에러 체크를 하지 않고 실행을 하는 인터프리터 언어적 특성으로 인해, 
자바스크립트의 document.getElementById('name') 가 html 내부 id가 name이란 태그가 생성되기도 전에 실행되므로
요소를 가져올 수가 없어 문제가 일어나는 것이다.

그러므로 아래와 같이 자바스크립트 태그를 문서의 뒤로 옮겨야만 하는데, 문제가 해결되기는 하지만 html 문서가 길어진다면,
내가 만든 자바스크립트가 아래쪽에 놓여있다면 휠을 내리기도 귀찮고 보기에도 안좋다.

window.onload

그렇기에 자바스크립트가 문서가 준비된 상황 이후에 발동하도록만 한다면 문서 앞에 선언해도 상관 없어지는데, 
바로 이런 것을 해주는 것이 window.onload() 메소드 인 것이다. 웹브라우저 자체를 담당하는 window라는 객체가 웹 문서를
불러올 때 문서가 사용되는 시점에 실행되는 onload라는 함수를 내가 다시 재정의 한다는 개념이다.

<html>
    <body>
        <script>
            window.onload = function() {
                let a = document.getElementById('name');
                a.style.color = "blue"
            }
        </script>
        
        <p id="name">hello</p>
    </body>
</html>

위와 같이 메서드를 재정의 해주면 되는데, 해당 함수 내의 코드 스크립트는
 웹브라우저 내의 모든 요소가 준비가 되어야 실행이 되도록 할 수 있다.

window.onload 문제점

하지만 window.onload() 메소드는 오로지 한번만 정의할 수 있다는 한계점이 존재한다. 만일 다른 위치의 <script> 태그에서 메소드를 
사용하려고 한다면, 이미 위에서 한번 정의했다면, 중복 처리가 되어 분담 사용이 불가능하다.

따라서 아래와 같이 addEventListener() 메서드를 통해 load 이벤트를 받는 식으로 구성하면 얼마든지 중복해서 사용이 가능하다.



<html>
    <body>
        <script>
            window.addEventListener('load', function() {
                let a = document.getElementById('name');
                a.style.color = "blue"
            })
        </script>
        
        <p id="name">hello</p>
    </body>
</html>


