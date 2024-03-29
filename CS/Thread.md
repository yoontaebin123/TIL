## **스레드(Thread)란?**

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdqBr2Z%2FbtriX7OJWvq%2FjBmQ6TMvLFWf65yQFLdNz0%2Fimg.png)

스레드(Thread)란 프로세스 내에서 실행되는 흐름의 단위 혹은 CPU 스케줄링의 기본 단위 라고 할 수 있다.   
스레드는 다음과 같은 특징을 가지고 있다.
+ 스레드는 각자 자신의 Stack 영역을 보유한다.
+ 스레드는 프로세스 내에서 Code, Data, Heap 영역을 공유한다.
+ 스레드를 생성하고 switching 하는 것은 inexpensive 하다.

<br>

## **스레드와 프로세스 차이**

스레드와 프로세스는 다음과 같은 차이들이 존재한다.
+ 프로세스는 각자 프로세스간의 통신에 IPC가 필요하다. 스레드는 스레드 간의 통신에 IPC가 필요하지 않다.
+ 각 프로세스는 Code, Data, Heap, Stack 영역을 각자 보유한다. 스레드는 Code, Data, Heap 영역은 공유하고 Stack 영역만 각자 보유한다.
+ 프로세스는 생성과 context switching에 많은 비용이 들어간다. 스레드는 생성과 context switching에 적은 비용이 들어간다.

프로세스 : 별도의 메모리 공간에서 실행   
스레드 : 동일한 프로세스 내의 공유 메모리 공간에서 실행

<br>

## **스레드의 장단점**

### **장점**

+ 다중 스레드의 경우 일부 스레드의 처리가 지연되더라도 다른 스레드에서 작업을 계속 처리할 수 있다.
+ 프로세스를 생성하여 자원을 할당하는 시스템 콜이 줄어들어 자원을 효율적으로 관리 할 수 있다.

### **단점**
+ 하나의 스레드에서 발생한 문제가 프로세스 전반에 영향을 미칠 수 있다.
+ 자원을 공유하기 때문에 동기화 문제가 발생할 수 있다.

<br>

## **스레드의 유형**
스레드는 크게 커널 영역과 사용자 영역으로 구분되는데, 이 중 멀티 스레드를 어느 영역에서 구현했느냐에 따라 사용자 수준 스레드와 커널 수준 스레드로 구분할 수 있다.

### **1.커널 수준 스레드(1:1 매핑 방식)**
스레드를 OS(커널)이 직접 관리(스레드 생성, 스케줄링 등등)한다.

![Image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FSc4OP%2FbtrAVtAl4C2%2FDQVavAV6XEIi368VhfzEzk%2Fimg.png)

#### **장점**
+ 커널이 각 스레드를 개별적으로 관리
+ 프로세스 내 스레드들이 병행 수행 가능
+ 하나의 스레드가 block 되어도 다른 스레드는 계속 작업 가능

#### **단점**
+ 커널 영역에서 스레드의 생성 및 관리
+ 문맥 교환으로 인한 오버헤드가 큼
+ 사용자 스레드보다 생성 및 관리 속도가 느림

### **2.사용자 수준 스레드(N:1 매핑)**
사용자 영역의 스레드 라이브러리로 구현하며, 라이브러리는 스레드의 생성, 스케줄링을 담당한다. 또한 커널은 스레드의 존재를 인식하지 못하기 때문에 커널의 개입을 받지 않는다.

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F5f3OL%2FbtrAYudzA4x%2FWGtSVkdwKeTRyoVEGWZ2Sk%2Fimg.png)

#### **장점**
+ 커널의 개입을 받지 않아 스레드의 생성 및 관리의 부하가 적음
+ 동일한 메모리 영역에서 스레드가 생성 및 관리되므로 속도가 빠름
+ 라이브러리를 통한 이식성이 높음

#### **단점**
+ 커널은 프로세스 단위로 자원을 할당하기 때문에 하나의 스레드가 block 될 시 모든 스레드가 대기해야 함(커널이 프로세스 내부 스레드를 인식하지 못하여 해당 프로세스를 대기상태로 전환 시키기 때문)

### **3.혼합 스레드(N:M 매핑 방식)**
사용자 수준 스레드와 커널 수준 스레드를 혼합한 구조이다. 일대일 방식과 다대일 방식의 문제점을 해결하기 위해 고안되었다.

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb20vhF%2FbtrAR8ihyMI%2FH0pIdFlMu98kktYcGxkQ6K%2Fimg.png)

#### **장점**
+ 프로세스 내 스레드들이 병행 수행 가능
+ 사용자는 원하는 수만큼 스레드 사용
+ 효율적이면서도 유연함
+ 스레드 풀링 기법을 통해 일대일 스레드 매핑에서의 오버헤드를 줄여줌

> ### 스레드 풀링
> 시스템이 관리하는 스레드의 풀을 응용프로그램에 제공하여 스레드를 효율적으로 사용할 수 있게 하는 방법.  
>  
> 미리 생성한 스레드를 재사용하도록 하여 스레드 생성 시간을 줄이고 시스템 부하를 덜어준다. 그리고 동시에 생성할 수 있는 스레드수를 제한하여 시스템의 자원 소비를 줄이고 해당 응용프로그램의 성능을 일정 수준으로 유지할 수 있다.


