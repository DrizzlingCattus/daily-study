# **[부스트캠프 6일차]**

## 주간 자기 회고 시간 중 느낀것

오늘은 오전 시간에 캠프에대한 설문 및 회고시간을 가졌다. 

매일매일 기록과 회고를 한 덕분에 오래 생각할 필요는 없었고, 캠프에 대한 설문 작성 위주로 시간을 할애하였다. 

그리고, 오전 후반 시간에는 주변 이들과 회고에 대한 공유를 하는 시간이 있었는데, 다들 이번 캠프 를 통해 많이 성장하고 있다는게 느껴졌다. 나 또한 짧은 기간이지만 큰 성장을 하였던거 같아 만족스럽다. 

나의 경우에는 지난 일주일 동안 테스트, 타입스크립트, linter 등 내가 평소에 잘 사용하지 않았던 기술들을 많이 활용할 수 있었다. 

또한, 코드를 완성한 뒤 리펙토링을 하는데 많은 시간을 할애할 수 있었고, 코드를 작성하는 기술이 많이 늘어난것 같다. 

더불어 코드를 간결하게 작성하고, 함수형 프로그래밍을 사용하는데 주요한 함수들을 잘 활용할 수 있게 된것 같다. 

## 챌린지 중 느낀것

### 디버깅

오늘은 월요일이라 여유를 갖고 챌린지에 임했다. 낮잠도 자면서, 천천히 코드를 작성하였다. 

오늘은 디버깅과 관련한 주제가 주어졌고, 코드를 수정하는 과제가 나왔는데 생각보다 많은것을 배울 여지를 준 과제였다. 

처음에 과제를 받았을 때는 단순히 코드를 수정하는 것인줄 알고 얕 보았지만,  과제를 조금 더 깊이 들어가니 실제 작업중에 자주 생길만한 문제들이 많이 보였고 generator와 같은 자주 사용하지 않던 기능을 조금 더 이해할 수 있었다. 

이번 기회를 통하여 VScode와 Debug 기능에 대해 명확하게 알 수 있었는데, 특히 다른 참가자 분께서 디버깅을 하는 경우 함수의 끝에 breakpoint 를 넣으면 내장 함수를 들어가지 않고 variable들을 볼 수 있다는 점을 알려주셔서 많은 도움이 되었다. 

코드를 그대로 남길 수 없어 자세한 사고 과정은 서술할 수 없지만, 내가 문제를 해결하며 주요하게 고민하였던 부분들에 대해 언급하겠다. 

나는 디버깅을 할때 두가지 부분에 대해 유의하며 진행했다. 

1. 내 코드 자체에 오류가 있을 경우(SyntaxError, TypeError 등)
2. 내 코드에 오류는 없지만, 로직이 잘못되어 원하는 결과가 나오지 않는경우

1번 문제의 경우에는 vscode의 linter를 설치하여 사용할 수 있고, 디버그 모드로 들어가 순회하며 오류를 찾을 수 있다. 

2번의 경우에는 디버깅 모드로 들어가 함수 전후로 Variables에 할당된 값을 비교 분석하여 체크한다. 

즉, 내가 지금 갖고 있는 문제가 코드 자체가 잘못 되었는지, 로직이 잘못 되었는지를 잘 고민하여야 한다. 

만약 로직이 잘못 되었는데, 코드의 오류를 찾는 것은 무의미하다. 차라리 디버깅 모드에서 코드를 빠르게 넘겨가며 내가 원하는 값들이 원하는 순간에 원하는 곳에 할당되어 있는지 파악하는게 옳다.

예를 들어,  다음과 같이 type 혹은 syntax 등의 코드에 대한 에러일때는 수정해야 할 부분이 명확해 진다. 

```js
  TypeError: a.Checker is not a constructor
      at Object.<anonymous>(...)
```

```js
  Exception has occurred: SyntaxError
  SyntaxError: Identifier 'msg' has already been declared
```

### 테스트

반면, 테스트는 잘 작성할 수 없었다. 이 부분은 다른 분들의 코드를 파악하며 학습해야 겠다. 

특히 generator와 같이 호출하는 횟수에 따라 반환값이 달라지는 경우는 어떻게 테스트를 해야 하는지 감이 오지 않았다 .

또한 함수들끼리 서로 왔다 갔다 하며 연결고리가 복잡하게 얽혀 있는 경우에도 테스트를 어떻게 진해해야 할지 감이 오지 않았다. 

만약 이 코드를 TDD로 작성하려 한다 해도 어떻게 해야 할지 모르겠다. 

이 부분들은 주말에 다시 한번 더 고민해봐야 겠다. 

## 회고

여유를 부리면 안되지만 여유를 부린 하루였다. 

열차가 지나가는 모습이 보이는 자리에 앉아 느긋하게 코딩하는 하루였다 .
