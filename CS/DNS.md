## **DNS**
DNS(Domain Name System)은 호스트의 도메인 이름을 호스트의 네트워크 주소로 바꾸거나 그 반대의 변환을 수행할 수 있도록 하기 위해 개발된 시스템. 특정 컴퓨터의 주소를 찾기 위해, 사람이 이해하기 쉬운 도메인 이름을 숫자로 된 식별 번호(IP Address)로 변환해 준다. DNS는 흔히 "전화번호부"에 비유 된다.

인터넷은 2개의 주요 이름 공간(Namespace)을 관리하는데, 하나는 도메인 네임 계층, 다른 하나는 인터넷 프로토콜(IP) 주소 공간이다. DNS는 도메인 네임 계층을 관리하며 해당 네임 계층과 주소 공간 간의 변환 서비스를 제공한다. 인터넷 네임서버와 통신 프로토콜은 도메인 네임 시스템을 구현하나. DNS 네임 서버는 도메인을 위한 DNS 레코드를 저장하는 서버이다. DNS 네임 서버는 데이터베이스에 대한 쿼리의 응답 정보와 함께 응답한다.

짧게 정리하자면 호스트 네임을 IP주소로 변환해주는 디렉터리 서비스이다.

### **DNS가 제공하는 추가 서비스 (주소 변환 외)**

#### **1. 호스트 엘리어싱 (host aliasing)**
복잡한 호스트 네임을 가진 호스트는 하나이상의 별명을 가질 수 있다. 이때 복잡하지만 실제 지정한 호스트 네임을 정식 호스트 네임(Canonical hostname)라고 하며 기억하기 쉽게 사용되는 별명들을 별칭 호스트 네임이라고 한다. DNS는 호스트의 IP주소뿐만 아니라 제시한 별칭 호스트 네임에 대한 정식 호스트 네임을 얻기 위해 이용될 수 있다.

#### **2. 메일 서버 엘리어싱 (mail server aliasing)**
제공된 별칭 호스트 네임에 대한 정식 호스트 네임을 얻기위해 메일 애플리케이션에 의해 수행된다.

예를 들어 메일 계정이 bob@relay1.west-coast.enterprise.com 이라고 할 때 bob@enterprise.com이 더 기억하기 쉽다. 이를 해석해주기 위해 사용된다.

#### **3. 부하 분산 (load distribution)**
DNS는 중복 웹 서버 같은 여러 중복 서버 사이에 부하를 분산하기 위해서도 사용되고 있다. 인기 있는 사이트는 여러 서버에 중복되어 있어서, 각 서버가 다른 종단 시스템에서 수행되고 다른 IP주소를 갖는다. 이때 여러 IP주소가 정식 호스트 네임과 연관되어 있다. DNS 데이터베이스는 이 IP주소 집합을 갖고 있다가 해당 호스트 네임에 대하여 DNS 질의를 하면 서버는 IP주소 집합 전체를 가지고 응답한다. 이때 응답되는 주소는 순환식(Round Robin)으로 보내며 이는 트래픽을 분산하는 효과를 낸다.

### **DNS의 동작 원리**
DNS의 간단한 설계로는 모든 매핑을 포함하는 하나의 네임 서버를 생각할 수 있다. 이러한 중앙 집중 방식에서, 클라이언트는 모든 질의를 단일 네임 서버로 보내고, DNS 서버는 질의 클라이언트에게 응답한다. 이러한 방식은 간단하지만 이러한 문제점들이 있다.

### **DNS를 중앙 집중 방식으로 구성할 때 생겨날 문제점**
1. 서버의 고장  
중앙 서버의 고장으로 전쳬 인터넷이 작동하지 못할 수 있다.

2. 트래픽 양  
단일 DNS가 모든 DNS 질의를 처리하는데 부하

3. 먼 거리의 중앙 집중 데이터베이스  
모든 클라이언트로부터 가깝지 않을 수 있다(멀리 있을수록 더 지연이 생긴다).

4. 유지관리  
모든 인터넷 호스트에 대한 레코드를 관리해야 하며, 거대한 DB를 업데이트하기 위해 잦은 UPDATE가 일어날 수 있다.

이러한 문제가 잠재되어 있어 DNS는 분산 구조로 설계되어있다.

### **DNS의 분산 계층 구조**
1. 루트 DNS 서버  
도메인 이름 공간의 최고점에 있는 정보를 보유한 네임서버로서 인터넷의 핵심을 담당하는 중요한 서버이다.

2. 최상위 레벨 도메인(TLD) 서버  
com, org, net, edu 같은 상위 레벨 도메인과 kr, uk, fr, ca 같은 모든 국가의 상위 레벨 도메인에 대한 서버가 있다. TLD 서버들은 책임 DNS 서버들에 대한 IP 주소들을 제공한다.

3. 책임 DNS 서버  
인터넷에서 접근하기 쉬운 페이지를 가진 기관은 호스트 네임을 IP주소로 연결시키는 공개적인 DNS 레코드를 제공한다.

### **로컬 DNS 서버**
로컬 DNS 서버는 계층구조에 속하지는 않지만 DNS 구조의 중심에 있다. ISP(Internet service provider)는 로컬 DNS 서버를 갖는다. 호스트가 ISP에 연결될 때, 그 ISP는 로컬 DNS 서버로부터 IP주소를 호스트에게 제공한다. 호스트가 DNS 질의를 보내면, 이 질의는 먼저 프락시로 동작하는 로컬 DNS 서버에게 전달되고, 그 로컬 DNS 서버는 이 질의를 DNS 서버 계층으로 전달한다.

![image](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile6.uf.tistory.com%2Fimage%2F9948BC465C305E061A2C59)

### **DNS 캐싱**
DNS는 지연 성능 향상과 네트워크의 DNS 메시지 수를 줄이기 위해 캐싱을 사용한다.  
질의 사슬에서 DNS 서버가 DNS 응답을 받았을 때 로컬 메모리에 응답에 대한 정보를 저장할 수 있다.  
예를 들어 로컬 DNS 서버가 이미 질의가 들어왔던 호스트 네임과 IP Address 쌍을 메모리에 저장해두었다고 하자. 다른 호스트로부터 같은 DNS 질의가 왔을 때, 로컬 DNS 서버는 호스트 네임에 대한 책임이 없어도 메모리에 있는 IP Address를 매핑하여 호스트에게 제공해 줄 수 있다.  
이 매핑된 데이터는 영구적인 것이 아니기 때문에 DNS 서버가 기간을 정하여 기간 이후에 저장된 정보를 제거한다(흔히 2일로 설정 한다고 한다.)