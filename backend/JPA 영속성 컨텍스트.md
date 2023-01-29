# JPA 영속성 컨텍스트
+ 엔티티를 영구 저장하는 환경이라는 뜻으로, 애플리케이션과 데이터베이스 사이에서 객체를 보관하는 가상의 데이터베이스 같은 역할을 한다.
+ EntityManager에 엔티티를 저장하거나 조회하면 EntityManager는 영속성 컨텍스트에 엔티티를 보관하고 관리한다.

<br>

```
entityManager.persist(entity);
```
+ Entity를 영속성 컨텍스트에 저장하는 코드이며, 해당 코드는 DB에 저장이 안 된 상태이다.(트랜잭션이 끝나야 DB에 반영을 한다)

## EntityManager
+ EntityManager는 영속성 컨텍스트 내에서 Entity들을 관리하고 있다.
+ EntityManager는 JPA에서 제공하는 interface로 spring bean으로 등록되어 있어 Autowired로 사용할 수 있다.
```
@Autowired
private EntityManager entityManager;
```
+ Query Method, Simple JPA repository는 직접적으로 entityManager를 사용하지 않도록 한번 더 감싸준 것이다.
+ spring jpa에서 제공하지 않는 기능을 사용하거나 특별한 문제가 있어서 별도로 customizing을 해야한다면 entityManager를 직접 받아서 처리한다.
+ EntityManager는 Entity Cache를 갖고 있다.

## 특징

<br>

![image](https://velog.velcdn.com/images%2Fseongwon97%2Fpost%2F1f89ead1-6910-407a-afd2-865eef68079f%2Fimage.png)

+ 영속성 컨텍스트는 논리적인 개념이다.
+ 영속성 컨텍스트는 엔티티 매니저를 생성할 때 하나 만들어지며, 엔티티 매니저를 통해서 영속성 컨텍스트에 접근하고 관리할 수 있다.
+ 스프링에서 EntityManager를 주입받아서 쓰면, 같은 트랜잭션의 범위에 있는 EntityManager는 동일 영속성 컨텍스트에 접근한다.

<br>

## 영속성 컨텍스트를 사용하는 이유
1. 1차 캐시(조회 기능을 높여준다)
2. 동일성 보장
3. 트랜잭션을 지원하는 쓰기 지연

<br>

### 1차 캐시
+ 영속성 컨텍스트 내부에는 캐시가 있는데 이를 1차 캐시라고 부른다.
+ 캐시는 Map의 형태로 만들어지며 key는 id값, value는 해당 entity값이 들어 있다.
```
// entityManager.find(엔티티 클래스 타입, 식별자 값);
User findUser = entityManager.find("User.class", "1L");
```
+ 1차 캐시에서 엔티티를 첫 번째로 찾는다. 해당 엔티티가 있으면 바로 반환해준다.
+ 1차 캐시에 없을 경우 데이터베이스에서 조회한다. 조회한 데이터로 엔티티를 생성해 1차 캐시에 저장 한다. 이후 엔티티를 반환한다.

<br>

### 동일성 보장
+ 영속성 컨텍스트는 영속 엔티티의 동일성을 보장한다.
    + 동일성은 값 뿐만 아니라 실제 인스턴스 자체가 같다는 뜻이다.

<br>

### 트랜잭션을 지원하는 쓰기 지연
```
entityManager.flush();
```
+ entity값을 변경하면 DB에 바로 업데이트 하지 않는다.
+ 트랜잭션 내부에서 영속 상태의 entity의 값을 변경하면 Insert SQL Query들은 DB에 바로 보내지 않고 쿼리 저장소에 쿼리문들을 생성해서 쌓아둔다.
+ 이후 entityManager의 flush()나 트랜잭션의 commit을 통해 보내지게 된다.

<br>

## 엔티티의 생명주기
![image](https://media.vlpt.us/images/neptunes032/post/ecd3b113-862f-4158-a208-e1eeec92d61d/image.png)

<br>

### 비영속
+ 엔티티 객체를 생성했지만 아직 영속성 컨텍스트에 저장하지 않은 상태를 비영속이라 한다.
```
//객체만 생성한 비영속상태 
    User user = new User();
```

<br>

### 영속
+ 엔티티 매니저를 통해서 엔티티를 영속성 컨텍스트에 저장한 상태를 말하며 영속성 컨텍스트에 의해 관리 된다는 뜻.
```
@Autowired
private EntityManager entityManager;
// Class내에 Autowired로 EntityManager추가

    //객체만 생성한 비영속상태 
    User user = new User();
    
    // 객체를 저장한 영속상태
    entityManager.persist(user);
```

### 준영속
+ 영속성 컨텍스트가 관리하던 영속 상태의 엔티티를 더이상 관리하지 않으면 준영속 상태가 된다. 특정 엔티티를 준영속 상태로 만드려면 em.datch()를 호출하면 된다.
```
// 영속 -> 준영속
    // user엔티티를 영속성 컨텍스트에서 분리하면 준영속 상태가 된다.
    entityManager.detach(user);
    // 영속성 콘텍스트를 비우면 관리되고 있던 엔티티들은 준영속 상태가 된다. (대기 상태에 있는 변경 데이터들도 삭제)
    entityManager.clear();
    // 영속성 콘텍스트를 종료해도 관리되던 엔티티들은 준영속 상태가 된다.
   	entityManager.close();
    
    // 준영속 -> 영속 
    // detach를 하여 준영속상태에 빠진 entity를 merge를 하면 다시 영속 상태가 된다.
    entityManager.merge(user); 
```
+ 준영속 상태에서는 1차 캐시, 쓰기 지연, 변경 감지, 지연 로딩을 포함한 영속성 컨텍스트가 제공하는 어떠한 기능도 동작하지 않는다. 식별자 값은 가지고 있다.

<br>

### 삭제
+ 엔티티를 영속성 컨텍스트와 데이터베이스에서 삭제한다.
```
 // user엔티티를 영속성 컨텍스트와 DB에서 삭제
    entityManager.remove(user);
```



