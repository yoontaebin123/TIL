## **캐시(Cache)**
Cache란 자주 사용하는 데이터나 값을 미리 복사해 놓는 임시 장소를 가리킨다. 아래와 같은 저장공간 계층 구조에서 확인할 수 있듯이, 캐시는 저장 공간이 작고 비용이 비싼 대신 빠른 성능을 제공한다.
![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FZdW35%2FbtqCbTDmFj5%2FAKbAUKiWseStnfD1Cd7gqk%2Fimg.png)

Cache는 아래와 같으 경우에 사용을 고려하면 좋다.
+ 접근 시간에 비해 원래 데이터를 접근하는 시간이 오래 걸리는 경우(서버의 균일한 API 데이터)
+ 반복적으로 동일한 결과를 돌려주는 경우(이미지나 썸네일 등)

Cache에 데이터를 미리 복사해 놓으면 계산이나 접근 시간 없이 더 빠른 속도로 데이터에 접근할 수 있다. 결국 Cache란 반복적으로 데이터를 불러오는 경우에, 지속적으로 DBMS 혹은 서버에 요청하는 것이 아니라 Memory에 데이터를 저장하였다가 불러다 쓰는 것을 의미한다. Enterprise급 Application에서는 DBMS의 부하를 줄이고, 성능을 높이기 위해 캐시(Cache)를 사용한다. 원하는 데이터가 캐시에 존재할 경우 해당 데이터를 반환하며, 이러한 상황을 Cache Hit라고 한다. 하지만 원하는 데이터가 캐시에 존재하지 않을 경우 DBMS 또는 서버에 요청을 해야하며 이를 Cache Miss라고 한다. 캐시는 저장공간이 작기 때문에, 지속적으로 Cache Miss가 발생하는 데이터의 경우 캐시 전략에 따라서 저장중인 데이터를 변경해야 한다.

### **캐시의 필요성**
![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbzw7IJ%2FbtqCaglKbIT%2FUpQYleNiW3pKQsOrzYLR5K%2Fimg.jpg)

위의 그래프는 Long Tail 법칙의 그래프이다. Long Tail 법칙은 20%의 요구가 시스템 리소스의 대부분을 사용한다는 법칙이다. 그렇기 때문에 20%의 기능에 Cache를 이용함으로써 리소스 사용량은 대폭 줄이고, 성능은 대폭 향상시킬 수 있다.

## **Local Cache vs Global Cache**

### **Local Cache**
+ Local 장비 내에서만 사용되는 캐시로, Local 장비의 Resource를 이용한다.
+ Local에서만 작동하기 때문에 속도가 빠르다.
+ Local에서만 작동하기 때문에 다른 서버와 데이터 공유가 어렵다.

### **Global Cache**
+ 여러 서버에서 Cache Server에 접근하여 사용하는 캐시로 분산된 서버에서 데이터를 저장하고 조회할 수 있다.
+ 네트워크를 통해 데이터를 가져오므로, Local Cache에 비해 상대적으로 느리다.
+ 별도의 Cache서버를 이용하기 때문에 서버 간의 데이터 공유가 쉽다.

## **읽기 캐시 전략**
캐시를 읽을 때 어떤 전략을 사용할지 결정한다.   
기본적으로는 캐시 먼저 조회 후 DB를 다시 확인한 뒤, 캐시를 갱신하는 방법이다.

### **Cache Aside**
![image](https://github.com/ParkJiwoon/PrivateStudy/raw/master/database/images/screen_2022_03_16_01_54_00.png)

1. 캐시에 데이터가 있는지 확인
2. 데이터가 존재하면 (Cache Hit) 해당 캐시 데이터를 반환
3. 데이터가 존재하지 않으면 (Cache Miss) 애플리케이션에서 DB에 데이터 요청 후 캐시에 저장하고 데이터를 반환

애플리케이션에서 가장 일반적으로 사용되는 캐시 전략이다.   
주로 읽기 작업이 많은 애플리케이션에 적합하다.   
Cache Hit의 경우 DB를 확인하지 않기 때문에 캐시가 최신 데이터를 가지고 있는지(동기화) 가 중요하다.   
캐시가 분리되어 있기 때문에 원하는 데이터만 별도로 구성하여 캐시에 저장할 수 있고 캐시에 장애가 발생해도 DB에서 데이터를 가져오는데 문제가 없다.   
하지만 캐시에 장애가 발생했다는 뜻은 DB로 직접 오는 요청이 많아져서 전체적인 장애로 이어질 수 있다.

### **Read Through**
![image](https://github.com/ParkJiwoon/PrivateStudy/raw/master/database/images/screen_2022_03_16_01_54_53.png)

1. 캐시에 데이터 요청
2. 캐시는 데이터가 있으면 (Cache Hit) 바로 반환
3. 데이터가 없으면 (Cache Miss) 캐시가 DB에서 데이터를 조회한 후에 캐시에 저장 후 반환

Cache Aside와 비슷하지만 데이터 동기화를 라이브러리 또는 캐시 제공자에게 위험하는 방식이라는 차이점이 있다.

마찬가지로 읽기 작업이 많은 경우에 적합하며 두 방법 다 데이터를 처음 읽는 경우에는 Cache Miss가 발생해서 느리다는 특징이 있다.

Cache Aside와는 다르게 캐시에 의존성이 높기 때문에 캐시에 장애가 발생한 경우 바로 전체 장애로 이어진다.

이를 방지하기 위해 Cache Cluster등 가용성 높은 시스템을 구축해두는 것이 중요하다.

### **읽기 캐시에서 발생 가능한 장애: Thundering Herd**
캐시 서버를 구축했다고 해서 아무런 문제가 없는 것은 아니다.

캐시 읽기 전략에서는 공통적으로 캐시 확인 -> DB 확인 순서로 이어지는데 이 과정에서 캐시에 데이터가 있으면 DB 확인을 생략하는 것으로 성능을 향상시킨다.

하지만 서비스를 이제 막 오픈해서 캐시가 비어있는 경우에는 들어오는 요청이 전부 Cache Miss가 발생하고 DB 조회 후 캐시를 갱신하느라 장애가 발생할 수 있다.

이를 회피하기 위해서 캐시에 데이터를 미리 세팅해두는 Cache Warm up 작업을 하거나 첫 요정이 캐시 갱신될 때까지 기다린 후에 이후 요청은 전부 캐시에서 반환하게 할 수 있다.

Cache Warm up 작업을 할 때 어떤 데이터를 넣느냐에 따라 마찬가지로 Cache Miss가 발생할 수 있기 때문에 자주 들어올만한 데이터의 예측이 중요하다.

## **쓰기 캐시 전략**
쓰기 요청 시 어떤 시점에 캐시 갱신을 하는지에 따라 나뉜다.
+ Write Around : 캐시를 갱신하지 않음
+ Write Through : 캐시를 바로 갱신
+ Write Back : 캐시를 모아서 나중에 갱신

### **Write Around**
![image](https://github.com/ParkJiwoon/PrivateStudy/raw/master/database/images/screen_2022_03_16_01_58_03.png)

1. 데이터 추가/업데이트 요청이 들어오면 DB에만 데이터를 반영
2. 쓰기 작업에서 캐시는 건들지 않고 읽기 작업 시 Cache Miss가 발생하면 업데이트 됨

캐시 쓰기 전략이라고 하기는 좀 애매하게 캐시를 전혀 건들지 않는다.

수정사항은 DB에만 반영하고 캐시는 그대로 두기 때문에 Cache Miss가 발생하기 전까지는 캐시 갱신이 발생하지 않는다.

Cache가 갱신된지 얼마 안된 경우에는 캐시 Expire 처리 되기 전까지 계속 DB와 다른 데이터를 갖고 있다는 단점이 있다.

만약 업데이트 이후 바로 조회되지 않을거라는 확신이 있다면 캐시를 초기화하여 Cache Miss를 유도하는 방법으로 보완할 수 있다.

### **Write Through**
![image](https://github.com/ParkJiwoon/PrivateStudy/raw/master/database/images/screen_2022_03_16_01_54_53.png)
1. 캐시에 데이터를 추가하거나 업데이트
2. 캐시가 DB에 동기식으로 데이터 갱신
3. 캐시 데이터를 반환

Read Through 와 마찬가지로 DB 동기화 작업을 캐시에게 위임한다.

동기화까지 완료한 후에 데이터를 반환하기 때문에 캐시를 항상 최신 상태로 유지할 수 있다는 장점이 있다.

캐시 및 DB를 동기식으로 갱신한 후에 최종 데이터 반환이 발생하기 때문에 전반적으로 느려질 수 있다.

새로운 데이터를 캐시에 미리 넣어두기 때문에 읽기 성능을 향상시킬 수 있지만 이후에 읽히지 않을 데이터도 넣어두는 리소스 낭비가 발생할 수 있다.

### **Write Back(Write Behind)**
![image](https://github.com/ParkJiwoon/PrivateStudy/raw/master/database/images/screen_2022_03_16_01_57_15.png)

1. 캐시에 데이터를 추가하거나 업데이트
2. 캐시 데이터 반환
3. 캐시에 있던 데이터는 이후에 별도 서비스(이벤트 큐 등)를 통해 DB에 업데이트

캐시와 DB 동기화를 비동기로 하는 방법이며 동기화 과정이 생략되기 때문에 쓰기 작업이 많은 경우에 적합하다.

캐시에서 일정 시간 또는 일정량의 데이터를 모아놓았다가 한번에 DB에 업데이트 하기 때문에 쓰기 비용을 절약할 수 있다.

다른 캐시 전략에 비해 구현하기 복잡한 편이며 캐시에서 DB로 데이터를 업데이트 하기 전에 장애가 발생하면 데이터가 유실될 수 있다.

### **Refresh Ahead**
자주 사용되는 데이터를 캐시 만료 전에 미리 TTL(Expire time)을 갱신한다.

캐시 미스 발생을 최소화 할 수 있지만 Warm Up 작업과 마찬가지로 자주 사용되는 데이터를 잘 예측해야 한다.