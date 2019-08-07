# [부스트캠프 18일차]

## 피어 세션 중 생각 정리

### [작업중인 쓰레드 죽이기]

- Worker pool을 하나의 배열로 관리하면, 쓰레드를 제거해야 할 때 어떤 쓰레드를 제거할지 알기 어렵습니다.
- 저는 workingPool(`workingBarista`)과 더불어 idlePool(`idleBarista`)를 생성하여 구분하였습니다.
- 기본적으로 idlePool에 생성된 모든 Worker가 저장되며,
- queue에서 FIFO의 순서에 따라 필요시마다 idlePool->workingPool로 worker가 이동합니다.
- 주기적으로 idle한 worker를 파악하여 작업을 제공하며
- 필요시에 idle한 worker를 제공할 수 있습니다.
- 또한 이렇게 분리시에, workingPool에 존재하는 worker가 임의의 이유로 강제로 terminate시켜야 할 경우, 바로 제거가 가능합니다.

```js
postMessage(sucess); // 작업을 시작하고

const idx = workingBarista.indexOf(barista); // 현재 worker의 index찾고

workingBarista.splice(idx, 1); // 현재 worker를 workingBarista에서 분리한 뒤

idleBarista.push(barista); // 현재 worker를 idleBarista로 추가합니다.
```

### [setTimeout 내부 동작 방식]

- setTimeout의 동작 방식은 JS를 실행하는 엔진의 구현방식에 따라 상이합니다.
- 웹에서는 web API로 추상화된 기능을 사용할 수 있고, Node에서도 동일하게 사용 가능합니다.
- 노드에서의 setTimeout event Loop 속의 Timers에 의해 제공됩니다.
    - Event Loop의 Timer phase에서 콜백으로서 setTimeout과 setInterval이 스케쥴되면
    - 목표하는 시간에 다달으면 반환됩니다.
    - 이 작업은 메인 쓰레드가 아닌, 별도의 쓰레드 풀 혹은 OS에서 처리 되며
    - 목표하는 실행/종료 시간은 정확하게 지정된 시간을 목표로 하지 않습니다.
    - 다만, **최소** 해당 시간을 지나는 것만 보장합니다.
    - 따라서, 이를 사용하여 strict하게 시간을 ms 단위로 조작하는것은 불가능 합니다.
    - 이러한 setTimeout과 setInterval 명시적으로 삭제하지 않으면 계속해서 살아있으므로 clear 해 줘야 합니다.
    - 브라우저에서의 setTimout은 브라우저가 사용하는 엔진에 따라 차이가 있습니다.

### [Service Worker]

- 웹 어플리케이션, 브라우저 그리고 네트워크 사이에 존재하는 프록시 서버로서의 역할을 갖습니다.
- 이는 효과적인 오프라인 경험을 제공하기 위해 설계 되었습니다.
- 프록시 서버로서 중간의 네트워크 요청을 가로체고, 이를 바탕으로 네트워크 연결 상황에 따라 적절한 행동을 취합니다.
- 이와 동시에 네트워크에서 캐싱을 하며 사용자의 오프라인 웹 경험을 원활하게 합니다.
- 이와 더불어 웹에서의 푸시 알림(push notification)과 background sync api로의 접근을 가능케 합니다.

## 챌린지 중 느낀것

### [ 구조에 대한 고민 ]

오늘 코드를 작성하며 이벤트 방식의 처리를 최대한 활용하려 노력했다. 

이벤트 방식과 옵져버 패턴 등에 아직 부족함이 많지만, 내 생각대로 정리하여 구현하였다. 

> 항상 꿈은 원대하다

사실 처음에는 RX를 학습하여 적용해볼까 라는 고민이 있었다. 

지금 생각하면, 시간도 없는데 큰일 날 뻔했다. 학습을 하는데만 하루는 후딱 지나갔을 것이고, 과제는 해결하지 못했을 것이다. 

그러나 RX를 접목해보고자 했던 나의 생각은 어느정도 일리가 있었던것 같다. 

데이터가 시간에 따라 흐르고, 그 데이터 들을 처리하는 것은 이번 학습 과정과 어느정도 유사한듯 하였다 . 그러나 고민을 더 해보니, 시간의 흐름에 따라 주문이 계속 들어오면, 어느순간 사라지는 주문이 생겨버릴 수 있을거란 생각이 들었다. 

그래서 RX는 시도하지 않았고, 추후에 제대로 학습하여 도전 해보기로 하였다. 

> Decoupling & Modulization

이 용어를 제대로 사용 하였는지는 잘 모르겠다. 

그러나 오늘 구현을 함에 있어서 이 두 단어를 계속 머릿속에 두며 코딩을 하는것 만으로도, 사고의 확장을 느낄 수 있었다. 

내가 항상 짜오던 코드는, 함수가 다른 함수를 꼬리 물듯 호출하며, 이를 통해 기능간의 종속이 생기곤 했다. 

이번 과제에서는 event 방식을 사용하여 두 기능 사이의 직접적인 연결을 끊으려 노력하였다. 

(DB와 같은 역할을 하는 작업 queue는 분리 할 수 없었고 공유해야 했다.)

예를 들어, 일을 시키는 `manager` 와 일을 수행하는 `worker` 의 연결고리를 메세지로만 사용하여, 최대한 느슨한 연결고리를 만들고자 하였다. 

잘 되었는지는 모르겠다. 하지만, 이렇게 고민하는것 만으로도 큰 발전이 있었다. 

`woker` 의 기능/데이터를 `manager`에서 최대한 접근하지 않으려 노력했다. 

예를 들어, 어제의 경우에는 manager에서 worker의 상태를 확인하기 위한 로직이 manager에 들어 있었다. 이 정보는 다른 기능들과 공유가 어려웠었다. 

오늘은, 하나의 array pool? 과 같은 형태로 별도의 모듈 형식으로 manager와 분리 시켰다. 

### [오늘 한 고민들 공유]

> [.....] 의 입장에서 A로 부터 받은 신호(message)를 바탕으로 [.....] 을하여 고객의 주문이 완료가 되었음을 알아야 한다. 이 [.....]은 어떻게 구현할까

1. 대시보드에 표현되는 숫자를 지속적으로 올리고, 완료가 되었음을 판단 하는것은 소비자의 역할로 둔다. 
    - 가장 바보같은 방법인듯 하다. 그냥 음료수를 쫙 깔아놓고, 손님이 알아서 알아채라는 논리였다.
2. 고객의 고유 id와 음료의 id를 별도로 부여한다. 고객별로 음료 주문 배열에서 ID를 하나씩 Pop 하여 길이가 0(숫자)이 되면 주문 완료가 될것이다. 
    - 이를 어떻게 확인할까?
    - setInterval로 주기적으로 길이를 확인할까?
    - 길이가 0이 될때 어떤 이벤트를 발생할까?
3. 음료의 고유 ID없이, 총 주문수의 count만 계산한다. 음료 준비가 완료 되면 신호를 보내고, 신호에는 고객의 ID가 포함 되어 있어 고객별로 음료의 개수를 확인 가능하다. 
    - 고객이 주문한 총 음료수 개수를 기억하고
    - 고객의 ID로 전달된 음료수의 수를 카운트 해서
    - 두 수를 빼서 0 이 되면 주문에 대한 처리를 완료한것

결국 3번의 로직으로 진행 하였다. 

![](https://github.com/sukjae/daily-study/blob/master/boost-camp/day18-1.jpg)
![](https://github.com/sukjae/daily-study/blob/master/boost-camp/day18-2.jpg)

## 회고

오늘도 과제를 하며 몇번 씩 펜을 던지고 싶은 마음이 들었다.

8:58분까지 push를 하며 최선을 다해 구현 할 수 잇는데 까지 하고자 하였다.

다행히 마지막 push에서 원하는 기능을 모두 추가할 수 있었다.

항상 드는 생각이지만, 포기하지 않는게 가장 중요하다.

부스트캠프 코딩테스트 때도 잘 풀리지 않아 몇번이나 포기하고자 하는 마음이 생겼었다.

그러나 결국 어떻게든 끝까지 고민하여 데드라인 근처에서 구현할 수 있었다.

.오늘도 8시까지도 4개정도의 기능이 구현되지 않았었고, 심지어 오류도 나기 시작했었다. 하지만 끝까지 컴퓨터흘 놓지 않았고, 해낼 수 있었다

항상 그래 왔던것 같다.

내가 남들보다 뛰어나지 않으니 남들보다 두 배로 뛰는수밖에 없다.

더이상 과거의 나처럼 자만하고, 게을러지지 말자

> 끝까지 가면 내가 다 이겨