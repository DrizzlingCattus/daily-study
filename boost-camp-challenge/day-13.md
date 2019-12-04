# [부스트캠프 13일차]

## 피어 세션 중 생각 정리
> 전날 구현한 코드에 대해 다시 한번 정리하는 시간을 갖습니다

### [Model-Controller]

> 기능 구현을 용이하게 하고, 구조화 하기 위해 Model/Controller/Util/Starte 로 파일구조를 나눴다.

- `Model`은 데이터에 직접 접근하는 모든 작업을 수행한다.
- `Model`에서 데이터는 로컬 변수를 사용했지만, DB와 같은 역할이 있을거라 가정하고 대부분의 에러를 호출한다.
- `Controller`는 전반적인 명령 수행을 위한 로직이 swtich 문으로 분기 되어있다.
- `Controller`에서는 직접 데이터를 접근할 수 없고, 오직 `Model`을 통해서만 전달받아 사용할 수 있다.
- `Controller`속에 `View`를 담당하는 부분이 존재하며, 이는 `View`로 변화를 시켜줘야 하지만, 시간상의 제약으로 처리하지 못했다.
- `Error`를 호출하는 쪽은 `Model`이므로, `Controller`에서는 필히 `try-catch`문으로 에러를 핸들링 해줘야 하는 입장이다.
- `Error`를 호출하는 입장과 사용하는 입장의 구분이 명확하니, 불필요한 `Error`의 중복이 발생되지 않았다.

### [Immutable]

> 모든 데이터를 처리하는 함수는 Immutable한 데이터의 흐름을 가져가도록 노력했다.

- 함수는 순수하게 작성하기 위해 노력했다.
- 이를 위해 사이드 이펙트를 최소화 하려 노력했고, 그 수단으로 Immutable한 데이터 접근을 수행하였다.
- 새로운 배열을 반환하는 `reduce`, `filter`, `map`등의 기능을 활용하였다.
- 편의를 위해 Shallow Copy를 통한 새로운 배열/객체(데이터)의 반환을 사용하였다.
- 그러나, 연산에 필요한 배열/객체(데이터)의 구조가 복잡해 질수록 이를 처리하기 위한 로직이 복잡해 졌다.
- spread-syntax를 최대한 활용하여 변수들의 사용을 최소화 하였지만, 여전히 코드를 읽기 난해하다.
- 이 부분을 해결하기 위해선
    - 추후에 별도의 immutable 라이브러리를 사용하거나
    - 배열/객체 데이터의 중첩된 구조를 분리하여 관계형으로(`key` 사용) 만드는 방법 등이 있을것 같다.

```js
this.state = [
  ...this.state.filter(x => x.folder_name !== folder_name),
  {
    ...copiedFolder[0],
    files: [
      ...copiedFolder[0].files.filter(x => x.file_name !== file_name),
      {
        ...copiedFile[0],
        status,
      },
    ],
  },
];
```

> 코드 예시: 해석이 난해하지만, 변수 사용을 줄여봤다.

### [상수의 활용]

> 데이터를 처리하는데 있어 오타등의 이유로 인한 에러를 방지하기 위해 상수 사용

- 계속해서 사용되고, 접근하기 위한 `key`값으로 사용되는 `String`형태의 데이터를 따로 상수로 관리하였다.
- 이와 같이 사용하면, `local`과 같이 의미는 같지만, 형태가 다른 문자열을 작성하는 실수를 방지할 수 있다.

```js
const LOCAL = 'LOCAL';
const REMOTE = 'REMOTE';
const WORKING = 'WORKING';
const STAGING = 'STAGING';
const COMMITED = 'COMMITED';
```

- 이와 더불어 위 상수들을 배열로 만들어, 옵션들의 선택의 폭을 줄였다.(enum과 같은 느낌?)

```js
const locationOptions = [LOCAL, REMOTE];
const statusOptions = [WORKING, STAGING, COMMITED];
//...
if(!statusOptions.includes(status)){
    //...  some logic
}
```

## 챌린지 중 느낀것

> 오늘 작성한 내용중 [자바스크립트 비동기] 에 대한 정리는 추가로 수정하여 데일리 스터디에 넣도록 한다. 

### 구현 전 학습

오늘은 JS에서 중요한 비동기에 대한 개념이 나왔다. 

일전에 학습한 내용이였지만, 어제처럼 섣불리 구현하고 싶진 않았다. 

어제는 기술에 대한 학습을 충분히 하지 않고,구현을 하는데 급급했다.  이로 인해서 어느정도 구현을 하였지만, 기억에 남는 지식이 얼마 없었다. 

그래서 오늘은 안내에 나온 학습목표를 위한 내용을 충분히 숙지하고, 구현을 시작하였다. 

좀 어려운 개념이라 구현을 너무 늦게 시작한 감도 없지 않아 있지만 (오후 3시쯤 시작),  어느정도 성공적으로 마무리 지었다. 

과제가 너무 어렵게 나오면 이런식으로 하지 못하겠지만, 가능한 학습과 구현의 밸런스를 맞춰가며 적당하게 타협을 보고 싶다.(너무 한쪽으로만 가고 싶지 않다)

앞서 언급 하였듯이, 오늘은 코드를 구현하기 전에 기술에대해 다시한번 조사를 시작하였다. 

일단, 내가 얕게 알고 있는 지식으로는 과제에서 나온 형태의 Event Loop에 대한 설명이 살짝 잘못 된 설명이라는 것을 알고 있다. 

이에 대해 확실히 정리하기 위해 계속해서 학습을 하였다. 

그러나 안타깝게도, 용어의 난해함과 내용이 너무 추상적이여서 충분히 이해를 하지 못했다. 

하지만, 이해하려 노력하는 과정에서도 많은 배움을 얻을 수 있었다. 

### 구현 하며 느낀점

`setTimeout` `setInterval` 등의 기능들을 활용하여 비동기/동기를 오가며 문제를 해결했다. 

남들은 신기하도록 쉽게 해결 하지만, 나는 구현에 큰 어려움을 겪었다 ㅠㅠ

그럼에도 구현을 하며 비동기에 대한 이해가 늘어 난것 같다. 

특히 오늘 느낀것중 가장 큰 배움은 다음과 같다. 

- 서로 다른 무한 루프를 돌리는 방법
- 서로 다른 무한 루프가 돌아가는 중에 다음 줄을 실행하는 방법
- 그러면서 비동기도 생각하는 방법

#### [서로 다른 무한 루프를 돌리는 방법]

이 부분은 `setInterval` 을 하면 된다. 

`while` 등의 방법으로 무한 루프를 돌리면, 루프 밖을 나가 다른것을 실행하러 나가면 이를 다시 돌아올 방법이 마땅치 않다. 

이 상황에서 `setInterval`을 하면, `clear` 하지 않는 이상 계속 해서 실행 되기 때문에 무한루프와 같은 효과를 낼 수 있고, 이는 여러개 사용이 가능하다. 

#### [서로 다른 무한 루프가 돌아가는 중에 다음 줄을 실행하는 방법]

앞서 설명한  맥락과 같다.  

그런데 여기서 다음 실행할 코드를 비동기라 생각하고, 이 코드에 대한 순서를 고려한다면 구현이 좀 어려워진다. 

위의 `setInterval` 은 비동기 호출로서 

- 다음에 동기가 나오면 동기를 먼저 실행하고,
- 다음에 비동기가 나오면, 나오는 순간 바로 큐로 올려버린다.

우리의 구현의 경우에는 두개의 무한루프를 돌리면서

두 개 모두 루프를 벗어나 다른것을 실행할 수 있어야 했고

그러면서 외부의 비동기 함수는 바로 큐로 올리면 안되고, 무한 루프 속 루프가 한차례 돌아간 후에 올려야 한다. 

여기서 마지막 부분에 대한 구현이 가장 어려웠다.  그 이유는

- 동시에 두개의 무한 루프를 돌리기 위해 `setInterval` 을 사용하면
    - 밖의 루프(`setInterval`) 와 내부의 루프 (`while` 등) 으로 구분되서 작동해야 한다.
    - 그런데, 밖의 루프가 interval time에 의해서 계속 실행되므로
    - 밖의 개수만큼 새로운 내부 루프가 생성된다.
    - 이로 인해 로직이 꼬이고, 진정한 무한루프에 빠져 버린다.....
- 만약에 밖의 루프(`setInterval`)만을 활용하여 코드를 작성하면
    - 외부에 있는 비동기 함수들이 바로 큐에 쌓여서
    - `setInterval` 과 시작선이 같아지고, 그것은 이번 구현 조건에 부합하지 않았다.

결국 나는 편법과 같은 방법을 사용해서 구현했고, 내일 다른 이들은 어떻게 구현했을지 기대 된다.

#### [나의 구현 방법 정리]

```js
function outterFunc(time = new Date()) {
  let count = 1 // 어떤 조건
  let lastTime = time; 
  // ...클로져로 내부 변수
  return function() {
    while (count > 0) {
      // ... some logic
      count -= 1;
      lastTime = new Date();
    }
    // 탈출 조건
    if (count === 0 && new Date() - lastTime > 5000) {
      // ...some logic
      process.exit();
    }
    // 현재 함수에서 벗어나 재귀로 새롭게 호출
    // 단, 이 함수는 setTimeout 을 통해 한번 비동기로 실행
    // 그러므로, 맨 처음 시작에서는 위의 코드로 동기적으로 실행되지만
    // 그 다음부터는 비동기로 실행되고, 이 이후에 나오는 코드를 큐에 쌓을 수 있다
    if (count === 0) {
      setTimeout(() => {
        outterFunc(lastTime)();
      }, 0);
    }
  };
}
outterFunc()()
```
## 오전 시간에 느낀것

### 위치 메서드

`indexOf` 를 사용시에 주의해야 할 부분이 있다. 

그 것은 이 연산이 `===` 연산을 수행하여 값을 반환한다는 점이다. 

즉, 우리가 서로 다른 배열, 객체를 비교 연산자로 제공하면, 이를 찾지 못한다. 

이를 사용하는데 주의하도록 하자. 

예시)

```js
const p1 = {name:'lee'}
const p2 = [{name:'lee'}]
const p3 = [p1]

p2.indexOf(p1) // -1 , 없음
p3.indexOf(p1) // 0 , index 0 에 존재
```

### 배열의 변환

배열의 기능을 이용하여 다양한 기본 자료구조를 구현할 수 있다. 

우리가 알고 있는 `push` `pop` `shift` `unshift` 를 사용하면 된다. 

stack 구조 : `push` + `pop`

queue(FIFO) 구조 : `push` + `shift`

LIFO 구조 : `pop` + `unshift` 

이때 주의해야 할 점이 있는데, `unshift` 연산자는 받은 인자를 한번에 처리한다. 

```js
const arr = new Array()
arr.unshift("fist","second")

arr //['first','second']
```

앞에서 하나씩 앞에 넣어서 second, first 순서가 될 수 있을거라 착각하면 안된다.

이 오해는 다음과 같은 방향으로 갈거라 오해해서 생겼다.

`a → [ ]` 

`→ [a]` 

`b → [a]` 

`→ [b,a]` 

결론은 아니다 다음과 같이 움직인다

`a,b → [ ]`

`→ [a,b]`

## 회고

오늘은 실수를 너무 많이 했다. 

master 브랜치에 issue를 올리고....

master 브랜치에 PR을 보내버렸다.....

안되는 날은 뭘 해도 안되나 보다. 

오늘 실수를 다시 반복하지 않도록 항상 조심하자. 

피곤하다는 말로 변명을 할 수 없다.