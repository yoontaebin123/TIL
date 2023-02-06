# Controller, Service, Repository

## Controller 란?
+ MVC에서 C에 해당하며 주로 사용자의 요청을 처리한 후 지정된 뷰에 모델 객체를 넘겨주는 역할을 한다.
+ 즉 사용자의 요청이 진입하는 지점이며 요청에 따라 어떤 처리를 할지 결정을 Service에 넘겨준다. 그 후 Service에서 실질적으로 처리한 내용을 View에게 넘겨준다.

<br>

## Controller를 사용하는 이유
만약 대규모 서비스중 A서비스, B서비스, C서비스 등등 있다고 하면 이 많은 종류의 서비스를 한 클래스를 만들어서 꽉꽉 몰아 처리할 게 아니라 Controller라는 중간 제어자를 만들어서 A서비스에 대한것은 A-Controller가 맡고 B서비스는 B-Controller 이런식으로 역할에 따라 설계를 하고 코딩을하면 개발비용이나 유지보수비용이 대폭 줄어들기 때문에 Controller를 사용한다.

<br>

## Spring에서 Controller사용법
스프링에서의 컨트롤러를 지정해주기 위한 어노테이션은 @Controller와 @RestController가 있다.

### 1. @Controller(Spring MVC Controller)
+ 전통적인 Spring MVC의 컨트롤러인 @Controller는 주로 View를 반환하기 위해 사용한다. 하지만 @ResponseBody 어노테이션과 같이 사용하면 RestController와 똑같은 기능을 수행할 수 있다.
```java
@Controller
public class Controllerprac {
    @GetMapping("/home") //home으로 Get요청이들어오면
    public String homepage(){
        return "home.html"; //home.html생성
    }
}
```

### 2. @RestController(Spring Restful Controller)Permalink
+ RestController는 Controller에서 @ResponseBody 어노테이션이 붙은 효과를 지니게 된다. 즉 주용도는 JSON/XML형태로 객체 데이터 반환을 목적으로 한다.
```java
@RestController // JSON으로 데이터를 주고받음을 선언합니다.
public class ProductRestController {

    private final ProductService productService;
    private final ProductRepository productRepository;

    // 등록된 전체 상품 목록 조회
    @GetMapping("/api/products")
    public List<Product> getProducts() {
        return productRepository.findAll();
    }
}
```

<br>

## Service 란?
자 Service를 이해하기 위해 큰 틀을 보면
1. Client가 Request를 보낸다.(Ajax, Axios, fetch등..)
2. Request URL에 알맞은 Controller가 수신 받는다. (@Controller, @RestController)
3. Controller는 넘어온 요청을 처리하기 위해 Service를 호출한다.
4. Service는 알맞은 정보를 가공하여 Controller에게 데이터를 넘긴다.
5. Controller는 Service의 결과물을 Client에게 전달해준다.

Service가 알맞은 정보를 가공하는 과정을 `비지니스 로직을 수행한다.`라고 한다.   
Service가 비지니스 로직을 수행하고 데이터베이스에 접근하는 DAO를 이용해서 결과값을 받아온다.

<br>

## DAO란?
+ 단순하게 페이지를 불러오고 DB정보를 한번에 불러오는 간단한 프로젝트의 경우 Service와 DAO는 차이가 거의 없을 수 있다고 한다.
+ DAO는 쉽게 말해서 Mysql 서버에 접근하여 SQL문을 실행할 수 있는 객체이다.

## DAO 와 JPA
Spring Data JPA는 매우 적은 코드로 DAO를 구현할 수 있도록 해준다. 즉 인터페이스를 만드는 것 만으로도 Entity(@Entity)클래스에 대한 insert, Update, Delete, Select 를 실행할 수 있게 해준다. 뿐만 아니라 인터페이스에 메소드를 선언하는 것 만으로 라이트한 쿼리를 수행하는 코드를 만드는것과 동등한 작업을 수행한다.   

그러면 JPA를 사용하면 DAO는 직접 구현을 안해도된다는 생각을 할 수 있다. 하지만 JPA가 만들 수 있는 코드는 매우 가볍고 쉬운 쿼리를 대체하는 것이라서 복잡도가 높아지면 사용하기가 매우 어렵다   

이때 JPA만으로만 사용한다면 수행능력이 SQL을 직접 사용해서 개발하는 것보다 못한 상황이 벌어 질 수도 있다. 그래서 JPA를 깊게 공부를 해서 JPA로 복잡도가 높은 쿼리를 짜거나 아니면 복잡도가 높은 곳은 DAO로 같이 사용한다고 한다.

![dao](https://velog.velcdn.com/images%2Fjybin96%2Fpost%2F7304b826-dcdb-4ec8-a980-b5b43519cbb8%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-11-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.13.53.png)

<br>

## Repository 란?
+ Entity에 의해 생성된 DB에 접근하는 메서드 들을 사용하기 위한 인터페이스이다.
+ @Entity라는 어노테이션으로 데이터베이스 구조를 만들었다면 여기에 CRUD를 해야하는데 이것을 어떻게 할 것인지 정의해주는 계층이라고 생각하면 된다.

<br>

## 결론
![result](https://velog.velcdn.com/images%2Fjybin96%2Fpost%2Faedf43fc-56fa-47b3-b591-a2dc8ce0d8fd%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-11-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.23.57.png)