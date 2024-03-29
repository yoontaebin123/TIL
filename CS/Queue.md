## **큐(Queue)**
![image](https://miro.medium.com/v2/resize:fit:1196/1*PMYRFmVecFT61P4aAh0g1g.png)

큐(Queue)는 스택(Stack)과 다르게 먼저 들어온 것이 먼저 나가는 선입선출로, FIFO(First In First Out)의 구조를 가지고 있다.

일반적으로 카페에서 먼저 와서 주문한 손님이 음료를 먼저 받고 나가는 것을 예시로 들 수 있다.

삭제 연산이 수행되는 곳을 프론트(front), 삽입 연산이 이루어지는 곳은 리어(rear)로, FIFO 구조를 위해서 스택과 다르게 큐의 한쪽 끝에는 삽입 작업이, 다른 한쪽 끝에서는 삭제 작업이 나뉘어서 이루어지고 있다.

큐는 리어(rear)에서 이루어지는 삽입 연산을 인큐(Enqueue)라고 부르며, 프론트(front)에서 이루어지는 삭제 연산을 디큐(Dequeue)라고 부른다.

### **큐(Queue)의 사용 사례**
+ 은행 업무
+ 대기열 순서와 같은 우선순위의 작업 예약 등
+ 서비스 센터의 대기시간
+ 프로세스 관리

### **큐(Queue)의 문제점**
큐(Queue)를 구현하고 사용할때 큐에서 데이터를 빼내는 함수를 쓰게되면 맨 앞에있던 값이 빠져나가게 되는데 이때 front가 한칸씩 뒤로 밀려나게 되면서 큐의 가용범위가 줄어들면서, 큐의 재사용 또한 불가능하게 된다.

만약에, 억지로 큐의 재사용을 하기위해서 front를 출력하고 front뒤의 인덱스 하나씩 앞당긴다고 하더라도 불필요한 연산이 너무 많아진다.