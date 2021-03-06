# [부스트캠프 8일차]

## 피어 세션 중 느낀것

### Immutable vs mutable

immutable를 염두에 두고 설계를 하면, 내부 상태를 원치 않는 상황에서 변화시킬 걱정을 하지 않고 코드를 작성할 수 있다. 우리가 내부의 상태를 변화시킬 때는, 우리가 원하는 상황에서만, 원하는 도구로만 변화를 시킬 수 있어야 한다. 

이와 같은 기법은 협업을 하며 코드를 작성할 때 큰 도움이 된다.  데이터가 흐르면서 계속해서 상태를 변화 시키다면, 이후에 해당 코드를 활용하거나 수정할 때 그 흐름을 파악하기 어렵고, 약간의 변화가 큰 오류를 만들어 낸다 .

### 함수만으로 구현하는데 어떤 장점과 단점이 존재하는가 ?

- 장점
    - 동시성 처리에 강점이 있다.
    - 코드가 간결해 진다.
    - 사이드 이펙트를 크게 고려하지 않아도 된다.
    - 유닛 테스트를 작성하기 편하다
- 단점
    - Data Consistency를 보장하기 위해 복잡함을 수반한다.
    - 간결한 코드를 읽기 위해서 학습이 필요하다.

## 챌린지 중 느낀것

### Docker

[docker hub에서 오피셜 docker ubuntu image](https://hub.docker.com/_/ubuntu)로 container를 생성하였다. 

[docker cheatsheet가 있으니 참고하자](https://www.docker.com/sites/default/files/Docker_CheatSheet_08.09.2016_0.pdf)

이렇게 생성해준 도커를 포트를 열고 이름을 지정해주는게 일반적이다. 

    docker run -dit -p 1200:22 -p 1100:80 --name boostcamp1 ubuntu:18.02
    

이렇게 run시켜준것은 stop을 한뒤 rm 하여 삭제 가능하다.

현재 활성화된 도커에 접속하기 위해 `run` 을 사용한다면, 새로운 컨테이너가 생길 뿐이니 주의하자. 

활성화된 도커 (`docker ps` 로 확인 후 )에 접속하기 위해서는 다음과 같은 코드를 사용하면 된다. 

    docker attach boostcamp1

설치는 간편 하였지만, 너무 컴팩트한 이미지를 다운받아서 그런지 기본적인 `sudo` 도 설치되어 있지 않아 추가 해 줘야 했다 .

    apt-get update && apt-get install -y sudo && rm -rf /var/lib/apt/lists/*

이제 apt-get도 최신버전이고, sudo도 설치 되었으니, 외부에서 ssh 연결을 하기 위한 프로그램을 설치해 주었다. 

    apt-get install openssh-server

근데 문제는 또 nano와 같은 편집기도 설치되어 있지 않더라. 그래서 설치했다. 

    apt-get install nano

위 openssh-server에는 루트계정의 로그인을 받아야 하는데 다음과 같이 설정할 수 있었다. 

    nano /etc/ssh/sshd_config
    
    ...
    PermitRootLogin yes 로 지정
    ...

이렇게 설치된 ssh 프로그램을 세가지 중요한 커맨드를 활용할 수 있다.

    service ssh start # ssh 시작하기
    service ssh stop # ssh 종료하기
    service ssh status # ssh 활성 상태 조회

start가 안되있으면 외부에서 ssh 연결이 안되는 것은 당연하다. 

이렇게 설정을한 뒤 주소와 포트를 확인하여 다음과 같이 접속이 가능하다.

    ssh -p 1200 root@localhost

아래와 같이 scp 를 통한 복사도 가능하다. (이유는 모르겠지만, scp에서는 -p가 대문자 -P이다!)

    scp -P 1200 ./backup/backup_${DATE}.zip lee@localhost:~/backup

근데 위 방법은 password를 요구하는데, 보통 ssh-key를 활용한다는 것을 염두에 두자. 

마지막으로, 도커가 활성화된 상태에서 추가적으로 포트를 바꿔주거나 설정을 바꿔줘야 하는 상황이 발생할 때 다음과 같은 방법을 사용할 수 있다. 일반적으로 생각할 수 있는것은, 도커를 죽이고 새로 생성(run)하는 방법인데, 다음과 같은 방법을 사용한다면 현재 도커 상태 그대로(유저 포함) 복+붙이 가능하다 .[[설명 링크]](https://stackoverflow.com/a/26622041)

1. [stop](https://docs.docker.com/engine/reference/commandline/stop/) running container

        docker stop test01

2. [commit](https://docs.docker.com/engine/reference/commandline/commit/) the container

        docker commit test01 test02

    **NOTE:** The above, `test02` is a new image that I'm constructing from the `test01` container.

3. re-[run](https://docs.docker.com/engine/reference/commandline/run/) from the commited image

        docker run -p 8080:8080 -td test02

`sudo` 를 설치해준 뒤 필요에 따라 유저를 생성할 수 있다. 

이번 기회에 도커의 다양한 커맨드를 잘 활용할 수 있었다. 

그러나, 이렇게 도커를 이미지로 받아서 하나하나 셋팅하기 보단 스크립트를 작성해서 자동화 해야 한다는 것을 안다. 이제 사용법에 익숙해 졌으니, 다음부터는 스크립트를 작성하도록 하겠다. 

### shell scripting

존재는 알고 있었지만, 직접 구현한적도 문법을 본적도 없는 것 이여서 너무나 생소했다. 

그냥 파이썬으로 되겠구나 싶었지만 이를 위한 언어와 문법이 존재 하더라. 

또, 파이썬이나 노드로 구현하면 해당 os에서 해당 언어를 위한 도구를 설치해줘야 하고, 속도의 문제도 있는것 같다. 생소하지만, shell scripting으로 작성해 보았다. 

결국 쉘 스크립팅은 우리가 터미널에 작성하는 커맨드들의 연장선 이더라. 우리가 자주 쓰는 커맨드의 뭉탱이를 그냥 쉘 스크립트에 기술해 놔도 순차적으로 잘 수행된다. 

순회를 하는것, 변수를 할당하는것 등 많은 신기한 작성법이 있었다. 

문법이 약간 뭐라할까... 동적이긴 한데 너무 스트릭한 부분도 있다. 특히 변수를 셋팅하고 값을 넣어줄 때 `=`가 변수명 바로 뒤에 붙어야 한다는게 신기했다. 이게 띄어쓰기가 들어가면 오류가 난다. 

또, `${}` 를 string에 넣으면서 다이나믹하게 string을 생성하는것은 뭔가 모던한거 같았다 .

또  array도 지원하여 신기하였다. 

string으로 처리되는것 같아 신기했다. 결국에 우리가 입력하는것도 string이니, 이 string의 조합을 잘 만들어서 무언가 액션을 취하는 것 같다 .

현재 나의 의문은, 많은 cli 도구가 유저에게 인풋을 받고, 다양한 부가 기능을 `-`나 `--` 등으로 받는데 이러한 장치들을 쉽게 만들 수 있는 프레임 워크가 없을까 라는 생각이 든다.

추가적인 학습을 위해 좋은 자료를 기록한다. 

[Bash shell scripting wikibooks](https://en.wikibooks.org/wiki/Bash_Shell_Scripting) 

bash가 무엇인지부터 스크립팅이 무엇인지 상세히 기술되어있다. 

[Bash scripting for beginners](https://linuxconfig.org/bash-scripting-tutorial-for-beginners)

GIF와 같은 사진이 포함되어 있어 설명을 이해하기 쉽다. 

[Your perfect kickstart to shell scripting](https://codeburst.io/your-perfect-kickstart-to-shell-scripting-857b81c0939b)

정말 쉽게 안내되어 있다. 

[shell scripting design guide by Google](https://google.github.io/styleguide/shell.xml)

구글에서 여러 언어에 대해 디자인 가이드를 만들어 놓은 것 같은데, 그중 쉘 스타일도 있다. 

[shell scripting array 사용법](https://stackoverflow.com/a/1951523)

[shell scripting에 색 입히기](https://stackoverflow.com/a/20983251)

### 추가적으로, cheat.sh

또, 다른 참가자 분이 공유해주신 꿀팁을 이용하여 다양한 예제를 손쉽게 활용할 수 있는 법도 배웠다. 

    curl cheat.sh/command_name

[cheat.sh](https://cheat.sh/)

이를 활용하면, 손쉽게 해당 커맨드의 활용 예시를 파악할 수 있다.!

## 회고

오늘은 주변에 많은 분들이 도커와 과제에 대해 물어봐 주셔서 뿌듯한 하루였다 .
