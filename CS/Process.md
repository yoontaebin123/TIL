## **프로세스(Process)란?**

프로세스는 실행 중인 프로그램(program)을 뜻한다.   
프로그램은 명령어들의 모음을 포함한 디스크에 저장된 파일이다.   
프로그램이 실행되면 이 프로그램의 명령어들과 데이터가 메모리에 적재되고 이것이 프로세스가 된다.

<br>

## **프로세스의 메모리 구조**

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F7HhNq%2FbtrffPKWXWR%2F9XCcQuqYwDBffANpQ5k3h0%2Fimg.png)

각 프로세스는 위 그림과 같은 구조를 갖는다. 각 영역은 다음과 같은 역할을 한다.
+ Code 영역 : 프로그램을 실행시키는 실행 파일 내의 명령어들이 위치하는 공간
+ Data 영역 : 전역변수, static 변수들이 위치하는 공간
+ Heap 영역 : 동적할당을 위한 메모리 영역(malloc(), new 등)
+ Stack 영역 : 지역 변수, 파라미터(함수에 전달되는 인자)가 위치하는 공간

<br>

## **프로세스 상태(Process State)**

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcBPyvJ%2FbtrfiGeHPSP%2FwLAs8w6lKmrb2PocWQP7d1%2Fimg.png)

+ new(상태) : 프로세스가 생성된 상태이다. OS 커널에 존재하는 Ready Quene에 올라가면 ready 상태가 된다.
+ ready(준비) : 프로세스가 CPU로부터 메모리 공간을 할당받길 기다리는 상태이다. 이때 프로세스 스케줄러에 의해 프로세스가 할당을 받게 되면 running 상태가 된다. 이것을 dispatch 라고 한다.
+ running(실행) : 명령어들이 실행되는 상태이다. 이때 interrupt(간섭)이 발생하면 ready 상태로 변한다. 실행을 끝마치면 exit(종료)되고 terminated 상태로 변한다. I/O(입출력)이나 event가 발생하면 waiting 상태로 변한다.
+ waiting(대기) : 특정 event가 발생하길 기다리는 상태이다. 이때 I/O나 특정 event가 완료되면 ready상태로 변한다.
+ terminated(종료) : 프로세스가 실행을 끝마친 상태이다.

<br>

## **프로세스 스케줄링(Process Scheduling)**

하나의 CPU를 가지고 있는 컴퓨터는 프로세스를 동시에 실행시킬 수 없다. 따라서 CPU는 고속으로 여러 프로세스를 일정한 기준으로 순서를 정해서 실행한다. 이러한 CPU 할당 순서 및 방법을 결정하는 과정을 프로세스 스케줄링이라고 하며 CPU 스케줄러가 이를 담당한다.