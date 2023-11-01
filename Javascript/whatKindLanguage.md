# Javasciprt는 어떤 언어인가

Javascript는 '싱글 스레드'이면서 '논 블록킹' 언어이다.

### 싱글 스레드 논 블록킹

 -자바스크립트는 비동기 처리를 통해 싱글 스레드이지만 블록킹 되지 않게 한다.
 하나의 요청이 완료될 때까지 기다리지 않고 동시에 다른 작업을 수행함으로써 블록킹 문제를 해결한다.

 ###싱글 스레드
 -스레드가 하나밖에 존재하지 않아 한번에 하나의 작업만 할 수 있다.

 ###스레드
 -어떠한 프로그램 내에서, 특히 프로세스 내에서 실행되는 흐름의 단위를 말한다.

 ###비동기 처리
 -특정 로직의 실행이 끝날 때 까지 기다려주지 않고 나머지 코드를 먼저 실행한다.
 멀티 스레드가 아닌 이유는 동시성 문제(동시에 공유된 자원에 접근하는 경우)를 해결하기 까다랍기 때문이다.