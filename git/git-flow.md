# Git-flow
+ Git flow전략은 소스코드를 관리하고 출시하기 위한 브랜치 관리 전략(branch management strategy) 중 하나이다.
+ Git flow에서 사용하는 브랜치의 종류는 5가지이며 크게 항상 유지되는 메인 브랜치(Master, Develop)와 일정 기간 유지되는 보조 브랜치(feature, release, hostifix)로 나뉜다.
    + **Master** : 제품으로 출시될 수 있는 브랜치
    + **Develop** : 다음 출시 버전을 개발하는 브랜치
    + **Feature** : 기능을 개발하는 브랜치
    + **Release** : 이전 출시 버전을 준비하는 브랜치
    + **Hostfix** : 출시 버전에서 발생한 버그를 수정하는 브랜치

+ 전반적인 git flow 전략을 소스코드는 다음 이미지와 같이 관리를 하게 된다.
![git flow](https://velog.velcdn.com/images%2Fseongwon97%2Fpost%2F5994c1a6-cdfd-4c73-84ea-842f9f7b001e%2Fimage.png)

+ 관리를 하는 순서
    1. repository를 생성하면 master브랜치에 위치할 것이다.
    2. 개발을 할 때는 develop 브랜치를 만들어 해당 브랜치에서 개발을 한다.
    3. develop브랜치에서도 특정 기능을 개발할 때는 feature브랜치를 생성하여 개발을 한다.
    4. feature브랜치의 개발이 끝나면 develop브랜치로 pull request를 보낸다.
    5. develop브랜치의 개발 리더 또는 동료 직원들이 해당 request를 확인하고 문제가 없다면 merge를 한다.
        + 5-1. merge이후에는 feature브랜치가 필요가 없어 삭제를 해준다.
    6. develop브랜치에서 어느정도 개발이 완료된다면 release브랜치를 생성하여 QA(품질검사)를 진행한다.
        + 6-1. release브랜치에서 발생한 버그들은 release브랜치에서 수정을 한다.
    7. QA를 통과하여 제품이 출시될 수 있다면 master와 develop브랜치로 merge한다.
    8. 마지막 master 브랜치에 버전 태그를 추가한다.

    > 만약 이미 출시되어 master로 관리되고 있는 버전에서 버그가 발생하여 빠르게 수정을 해야하는 상황이 발생한다면 master브랜치에서 hostfix브랜치를 생성하고 오류를 수정하고 master와 현재 개발중인 develop브랜치에 반영을 해준다.
