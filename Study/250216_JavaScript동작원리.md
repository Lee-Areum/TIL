## JS의 엔진
JavaScript 엔진이란 JavaScript를 실행하는 프로그램 또는 인터프리터

### 주요 엔진 종류
- V8(Chrome, Node.js)
	구글에서 개발한 오픈소스 엔진
    C++로 개발된 빠른 실행속도와 높은 성능을 가진 엔진
_-> 속도향상을 위해 인라인 캐싱 등 최적화 기법을 사용_ 
    주로 Chrome 브라우저나 Node.js 런타임 환경에서 사용
    안드로이드 브라우저에도 탑재되어있음
 
 - SpiderMonkey(FireFox)
 Mozilla에서 개발한 엔진
 최초의 JS엔진으로 안정성과 호환성에 중점을 둔 엔진
 주로 Firefox 브라우저에서 사용
 
 - JavaScriptCore(Safari)
 Apple에서 개발한 엔진, MacOS 및 IOS 환경의 Safari 브라우저에서 사용
 
 그외에도 Chakra(Microsoft), Hermes(React Native) 등이 있다.
 
 ### JavaScript 엔진의 주요 구성 요소
 ### 

- JS 엔진은 Memory Heap과 Call Stack으로 구성된다.
 ![](https://velog.velcdn.com/images/lee_areum/post/5b8989b9-df7f-4767-ac0c-a980ff18d3e6/image.png)

 #### 메모리 힙(Memory Heap)
 - 메모리 할당이 발생하는 위치
 - 동적으로 생성되는 데이터를 저장하는 메모리 공간임
 
 #### 콜 스택(Call Stack)
 - 코드가 실행될 때 스택 프레임(프로그램이 어느 위치에 있는가)를 기록하는 데이터 구조
 - 스택 형태로 동작하고 함수가 호출될 때 스택에 추가되고 실행 종료 시 스택에서 제거
 - **FILO**(first in Last out) 구조
 - Call Stack에서 실행하는 프로그램 단위를 스택 프레임 이라고 한다.
 
 ### 런타임
 JS 런타임 환경은 JS 실행 외에도 추가적인 기능들(api)을 사용한다.
 DOM, AJAX, setTimeout 등도 엔진에 포함되지 않은 기능들이다.
 
 여기서 나오는게 이벤트 루프(Event Loop)이다.
 
 #### 이벤트 루프(Event Loop)
 Event Loop는 JS 엔진과 런타임 환경의 중간 역할로
 싱글 스레드인 JS 엔진을 보조하여 비동기 작업을 처리하는 역할을 한다.
 
 이벤트 루프는 비동기 작업(네트워크 요청, 사용자 이벤트 등)이 완료될 때까지 기다렸다가 완료되면 해당 작업의 콜백 함수를 콜백 큐에 추가한다.
 ![](https://velog.velcdn.com/images/lee_areum/post/c7cdb50e-f6b9-4956-932f-f04e3e6d5e41/image.png)
** 콜백큐 **
비동기 작업의 결과나 나중에 실행되어야 하는 작업들이 대기하는 공간 **FIFO**(first in first out) 구조

<br>

_콜백 큐에는 또 세가지 종류가 있는데_
- **테스크 큐(Task Queue)** : 일반적으로 매크로 태스크(Macrotask Queue)라고도 불림,
 setTimeout, setInterval 등의 비동기 작업
- **마이크로태스크 큐(Microtask Queue)**: 프로미스(Promise)의 콜백 함수나 async / await
- **애니메이션 프레임(Animation Frames)**: 브라우저 환경에서 화면을 업데이트하는 작업, ex) requestAnimationFrames
 
 마이크로테스크 큐 > 애니메이션 프레임 > 테스크 큐 순으로 우선순위를 가지고 있음.
 
 이런 우선순위를 실행하는 작업을 이벤트 루프가 처리한다.
 
 
<br>

### 이벤트 루프(Event Loop)의 동작 과정
 이벤트 루프는 콜 스택과 콜백 큐를 지속적으로 확인하며, 콜 스택이 비어있으면 콜백 큐에서 가장 오래된 작업(FIFO)을 꺼내서 콜 스택으로 옮긴다.
 
이미지와 함께 보도록 하자.
 
1. **콜 스택(Call Stack) 확인**: 먼저 현재 콜 스택이 비어 있는지 확인하고
만약 콜 스택에 아직 처리되지 않은 함수가 있다면, 해당 함수가 완전히 실행될 때까지 대기한다.
![](https://velog.velcdn.com/images/lee_areum/post/689cf568-1c66-40eb-8dda-bfd87269f99d/image.png)
2. **콜백 큐(Callback Queue) 확인**: 만약 콜 스택이 비어 있다면, 콜백 큐를 확인한다.
_콜백 큐에는 웹 API 등에서 생성된 콜백 함수들이 대기한다._
![](https://velog.velcdn.com/images/lee_areum/post/e56dd277-5ce8-4d13-8602-4413e595fa6e/image.png)
3. **함수 이동**: 콜백 큐에서 가장 오래된 함수를 꺼내서 콜 스택으로 옮긴다.
![](https://velog.velcdn.com/images/lee_areum/post/50768979-8426-49c0-b704-154046515863/image.png)
4. **함수 실행**: 해당 함수가 콜 스택에서 실행되고 실행이 끝나면 콜 스택에서 빠져나간다.
위 과정들을 프로그램이 종료될 때까지 반복한다.

<br>

### 예시
이제 여러 예시 코드들에 따른 JS엔진의 동작과정을 살펴보겠다.

```
function greet() {
  return 'Hello!';
}

function respond() {
  return setTimeout(() => {
    return 'Hey!';
  }, 1000);
}

greet();
respond();
```
1. 먼저 great() 함수를 호출하고(콜 스택에 넣고 실행한다.)
다음으로 respond() 함수를 호출한다. 
2. setTimeout 함수와 respond 함수가 콜 스택에서 빠져나오면서 값을 반환하고
 setTimeout에 전달한 콜백 함수만 빠져나와 웹 api로 전달된다.
 웹 api에서는 setTimeout 함수의 두 번째 인수였던 1초 만큼 실행된다.
 3. 1초가 지나면 콜백 큐(Task Queue)로 돌아간다.
 4. 이벤트 루프가 콜 스택이 비어있는지 확인하고 콜백 큐에 있는 콜백 함수를 콜 스택에 추가한다.
 5. 콜백 함수는 콜 스택에 추가되고, 호출된 후 갑슬 반환하여 콜 스택에서 빠져나가게 된다.
<br>

관련 예시는 아래 블로그에 잘 기입되어 있다.
https://yong-nyong.tistory.com/71

예시를 보면서 async await는 예상과 다른 순서로 진행되었다.
async 함수 내에서 await를 만나기 전까지는 코드가 순차적으로 실행되지만
![](https://velog.velcdn.com/images/lee_areum/post/34ea8691-639b-4ad3-9ec7-b8d4be78f5ca/image.png)
await 함수를 만나는 순간 await 함수 내 동작(Promise 객체 반환)을 실행하고 async 함수는 일시정지되서 마이크로 테스크 큐로 이동된다.


 
 #### 참고
 - https://medium.com/sessionstack-blog/how-does-javascript-actually-work-part-1-b0bacc073cf
 - https://medium.com/sessionstack-blog/how-javascript-works-inside-the-v8-engine-5-tips-on-how-to-write-optimized-code-ac089e62b12e
- 출처 : https://yong-nyong.tistory.com/71 [💻용뇽 개발 노트💻:티스토리]
