이벤트 기본 동작 취소하기

웹브라우저의 구성요소들은 각각 기본적인 동작 방법을 가지고 있다.

1. 텍스트 필드에 포커스를 준 상태에서 키보드를 입력하면 텍스트가 입력된다.
2. 폼에서 submit 버튼을 누르면 데이터가 전송된다.
3. a 태그를 클릭하면 href 속성의 URL로 이동한다.
4. 그 밖의 브라우저 기본 단축키 (복사 기능이나, f12, 엔터 같이, 키를 누르면 기본적으로 동작하는 기능)

이러한 기본적인 동작들을 기본 이벤트라고 하는데, 사용자가 만든 이벤트를 이용하여 이러한 기본 동작을 취소하거나 조작 할 수 있다.
다음 3가지 방법을 소개해본다.

inline 방식

onclick의 속성값에 이벤트의 리턴값이 false이면 기본 동작이 취소된다.

<p>
    <label>prevent event on</label>
    <input id="prevent" type="checkbox" name="eventprevent" value="on" />
</p>
<p>
    <a href="http://opentutorials.org" onclick="if(document.getElementById('prevent').checked) return false;">opentutorials</a>
</p>
<p>
    <form action="http://opentutorials.org" onsubmit="if(document.getElementById('prevent').checked) return false;">
            <input type="submit" />
    </form>
</p>

property 방식

함수의 리턴 값이 false이면 기본 동작이 취소된다.

<p>
    <label>prevent event on</label>
    <input id="prevent" type="checkbox" name="eventprevent" value="on" />
</p>
<p>
    <a href="http://opentutorials.org">opentutorials</a>
</p>
<p>
    <form action="http://opentutorials.org">
            <input type="submit" />
    </form>
</p>

document.querySelector('a').onclick = function(event){
    if(document.getElementById('prevent').checked)
        return false;
};

document.querySelector('form').onclick = function(event){
    if(document.getElementById('prevent').checked)
        return false;
};

addEventListener 방식

이 방식에서는 이벤트 객체의 e.preventDefault() 메소드를 실행하면 기본 동작이 취소된다.

<p>
    <label>prevent event on</label>
    <input id="prevent" type="checkbox" name="eventprevent" value="on" />
</p>
<p>
    <a href="http://opentutorials.org">opentutorials</a>
</p>
<p>
    <form action="http://opentutorials.org">
        <input type="submit" />
    </form>
</p>

document.querySelector('a').addEventListener('click', function(e){
    if(document.getElementById('prevent').checked) 
        e.preventDefault(); // 링크 동작 금지
});

document.querySelector('form').addEventListener('submit', function(e){
    if(document.getElementById('prevent').checked) 
        e.preventDefault(); // 폼 submit(페이지갱신) 동작 금지
});
