# [부스트캠프 20일차]

## 피어 세션 중 생각 정리

### [테스트 코드]

- 테스트 코드를 효율적으로 관리하기 위해 Jest 라이브러리를 사용하여 구현하였다.
- 모든 모듈별로 테스트를 독립적으로 구성하여, 유지보수를 용이하게 하였다.

### [Node의 URL]

- Node의 URL클래스를 모방하여 작성하였다.
- Node의 URL클래스가 표현하는 표기법을 따르려 노력하였고(key값), `_URLSearchParams`와 같은 부가적인 기능을 추가 함으로서 Node의 URL을 이해하고자 하였다.

### [isEqual]

- `isEqual`메소드에서는 단순히 같고 다르고를 비교하는것을 넘어서서
- `[identical,different,similar]` 로 구분하였다.
- `similar`의 경우에는, URL의 구성요소중 어떤 부분이 같은지(`true`), 다른지(`false`), 혹은 존재하지 않는지(`undefined`)에 대한 정보를 객체로서 추가 제공한다.

## 금요일 프로젝트 중 느낀것

오늘도 프로젝트가 쉽지는 않았다. 

일단 몇가지 어려웟던 점을 요약하면

- 2주차에서 진행한 서버구성 & 화면 인식 기능과 3주차에서 진행한 프런트엔드 & 출석체크 기능사이에 연결고리가 전혀 없었다.
- 두 주차의 과제는 서로 연결을 위해 만들어지지 않았기 떄문에, 이를 연결하는데 크게 어려움을 겪었다.
- 이 두개를 합치는 작업과, 새로운 기능을 추가하는 작업을 해야하는데, 사공이 많아서 그런지... 자꾸 배가 산으로 갔다.

이러한 어려움으로 인해서 팀을 나누고 싶어도 나누기 어려웠고, 

새로운 기능을 추가하기도 어려웠다. 

내 머리속에는 이를 구성할 방법들이 떠오르는데... 이 부분을 나만 알것이 분명하고, 내가 혼자 하면 그건 팀플이 아니게 되기 때문에 말하기 조심스러웠다. 

결국, 타협을 보고 최소한의 기능만 유지한체 시간안에 구현할 수 있는 방향으로 갔다. 

또, 안타까웠던 부분은 소통이 어려웠다는 점이다. 

한번 얘기를 할때, 다같이 모여서 딱 끝내고 과제를 나눠 진행 해야하는데, 

이게 잘 안되고 소수만 계속 모이니 주제가 한 방향이 아니라 여러 방향으로 나눠지곤 했다. 

또, 이번 팀에서는 주도를 하고자 하시는 분들이 많아 보여서 내가 주도의 역할을 내려 놨었는데, 이 또한 잘 되지 않았다. 

역할이 잘 분배가 되지 않으니, 쉬는 사람들이 계속 생기게 되었다. 

할거는 많은데, 자원을 효율적으로 활용하지 못했던것 같다. 

그럼에도 불구하고, 후반에 모두가 힘을 합쳐 잘 마무리 지을 수 있었다. 

특히, 팀원 분들중에 말을 잘 하시는 분들도 많이 있으셨고, 

맡은 일을 다 끝내신 분들 덕분에 마감 시간에 무언가 제품은 나올 수 있었던것 같다. 

### 내가 혼자 (혹은 기술스택이 비슷한 사람) 했다면 어떻게 진행 했을까?

일단, 사람을 인식하는 로직과 같이 이미 만들어진 기능들은 최대한 활용하는 식으로 갔을 것 같다. 

저장소로는 S3를 사용하여 주기적으로 사람들의 사진을 저장하고, 

그 사진에 대한 정보(링크)와 간단한 출석 관련 정보는 DynamoDB에 저장을 했을 것 이다. 

프런트는 내가 익숙한 ReactJS로 후딱 짤 수 있을것이다. (화면이 굉장히 간단하다. )

이를 모두 빠르게 작업하기 위해서 Amplify를 사용하여 연결하였을 것이다. 

시간이 조금 남거나 팀원이 있었다면, 얼굴 인식 서비스(구글, AWS 등)을 이용하여 간단하게 얼굴과 이름을 매핑했을 것이다. 

> 결국 있는 기술을 잘 활용 하는게 관건이므로!

## 회고

오늘로서 부스트캠프 챌린지 과정이 종료 되었다.

오늘도 정신없이 오전부터 오후까지의 작업을 하니, 끝나는게 실감이 잘 안간다. 

당장 다음주 월요일부터 다시 성수로 갈거 같은 느낌이다. 

오늘은 또한 나의 성장을 느낄 수 있는 하루였다. 

과거의 나 같았으면, 팀 프로젝트 중에 원하는데로 잘 안되면 답답하다고 혼자 따로 했었을 것이다. (실제로도 몇번 그랬었다. )

하지만, 팀 프로젝트의 진정한 의미와 가치를 아는 지금, 그런 짓을 하지는 않는다. 

최대한 팀원에게 설명하고, 같이 하려 한다. 그래야 배우는게 있는 과정이므로.

그럼 점에서 나에게 뿌듯 하였다. 

(물론, 직장 같은 곳에 가면 성과와 시간이 중요하므로, 어떻게든 혼자 캐리하기 위해 뛰겠지만...)

오늘도 배울수 있어 감사한 하루다.
