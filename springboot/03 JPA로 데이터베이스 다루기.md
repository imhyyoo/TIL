# 03. JPA로 데이터베이스 다루기

## 3.1 JPA의 등장 배경

> **SQL Mapper**

SQL 구문으로 직접 RDBMS에 질의하여 결과 값을 객체에 매핑시켜주는 것이다. `SQL을 통해서만` 데이터베이스에 저장하고 조회할 수 있다. 

**관계형 데이터베이스**는 `어떻게 데이터를 저장할지` 에 초점을 맞추는 기술이고 객체지향언어는 메세지를 기반으로 `기능과 속성을 한곳에서 관리` 하는 기술이다. 

```java
Employee emp = sel_emp(); 
Department dept = emp.getDeptno();
```

`sel_emp` 메서드를 통해 직원 한명을 emp 객체에 저장하고, dept 객체에 emp 객체가 가진 `Department` 를 저장한다. 직원과 부서는 부모자식 관계임을 확인할 수 있다.  여기에 데이터베이스가 추가되면 다음과 같다. 

```java
Employee emp = empDao.sel_emp(); 
Department dept = deptDao.sel_dept(emp.getDeptno());
```

직원 객체에는 부서 번호가 저장되어있다. 데이터베이스에서 `직원 데이터를` 먼저 가져온 후, 부서 번호를 통해 다시 데이터베이스에서 해당 직원의 부서 데이터를 가져온다. 

직원과 부서를 따로따로 조회하기 때문에 직원과 부서가 `어떤 관계` 인지 알 수 없다. 이런 문제를 해결하기 위해 `JPA` 가 등장하였다.

> **ORM 기술인** **JPA**

`ORM`은 객체와 관계형 데이터베이스에 저장된 데이터를 매핑해준다. ORM을 이용하여 객체간의 관계를 표현하고 SQL문을 사용하지 않고 RDBMS에 접근할 수 있다. 

## 3.2 스프링 부트에서 JPA 사용하기

- JPA를 사용하기위해 의존성을 등록해야한다.

```java
compile('org.springframework.boot:spring-boot-starter-data-jpa')
compile('com.h2database:h2')
```

> **도메인 객체 생성**

```java
@Getter
@NoArgsConstructor
@Entity
public class Posts {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(length = 500, nullable = false)
    private String title;

    @Column(columnDefinition = "TEXT", nullable = false)
    private String content;

    private String author;

		@Builder
    public Posts(String title, String content, String author){
        this.title = title;
        this.content = content;
        this.author = author;
    }
}
```

1. `@Entity` : `JPA` 어노테이션이다. POSTS 클래스를 DB 테이블과 매칭하기 위해 선언하는 어노테이션이고, 이를 `Entity` 클래스라고 한다. 

    기본 값으로 클래스의 이름을 언더스코어 네이밍으로 테이블 이름을 매칭한다.

    ex) `[HelloWorld.java](http://helloworld.java)` → `hello_world table`

2. `@Id` : 해당 테이블의 PK 필드를 나타낸다. PK는 주로 `Long` 타입의 Auto_increment를 추천한다.
3. `@GeneratedValue` : PK 생성 규칙을 나타낸다. `GenerationType.IDENTITY` 옵션을 추가해야 auto increment 된다. 
4. `@Column` : 테이블의 칼럼을 나타낸다. 선언하지 않아도 클래스의 필드는 자동으로 칼럼이 된다. 옵션이 필요한 경우 선언해준다. 문자열의 경우 `varchar(255)` 가 기본값인데 사이즈를 늘리거나 타입을 TEXT로 변경하고 싶을 때 사용한다. 
5. `@Builder` : 해당 클래스의 빌더 패턴 클래스를 생성. 생성자 상단에 선언 시 생성자에 포함된 필드만 빌더에 포함한다.
6. `**@Entity` 클래스에서 절대 `Setter` 생성하지 않는다. 필드 값의 변경이 필요한 경우 목적과 의도를 표시해야한다.** 

> **Setter 없이 값을 DB에 저장하는 법**

- 기본적인 방법

    생성자를 통해 DB에 삽입하고 변경이 필요한 경우 `해당 이벤트` 에 맞는 `public 메서드` 호출

- `@Builder` **사용**

    생성 시점에 값을 삽입. 어떤 필드에 어떤 값 채울지 명확하게 인지가능. 

> **JPA에서 DB 접근방법(JPA의 DAO)**

```java
public interface PostsRepository extends JpaRepository<Posts, Long> {}
```

JPA에서는 DAO를 `Repository` 라고 부르며 인터페이스를 생성하는데 이 인터페이스가 `JpaRepository<Entity클래스, PK 타입>` 을 상속하면 기본적인 `CRUD` 메서드가 자동으로 생성된다.  `@Repository` 선언이 없어도 된다. `Entity` 클래스와 `Entitiy Repository` 가 **반드시 같이 위치**해야한다. 

- `SpringDataJpa` 에서 제공하지 않는 메서드는 쿼리로 작성 가능하다. 다음과 같이 작성하면 된다.

    ```java
    public interface PostsRepository extends JpaRepository<Posts, Long> {
    		@Query("SELECT p FROM Posts p ORDER BY p.id DESC")
        List<Posts> findAllDesc();
    }
    ```

```java
		@After
    public void cleanup(){
        postsRepository.deleteAll();
    }

		@Test
    public void 게시글저장_불러오기(){
        String title = "테스트 게시글";
        String content = "테스트 본문";
        postsRepository.save(Posts.builder()
            .title(title)
            .content(content)
                .author("imhyyoo@gmail.com")
                .build());

        List<Posts> postsList = postsRepository.findAll();

        Posts posts = postsList.get(0);
        assertThat(posts.getTitle()).isEqualTo(title);
        assertThat(posts.getContent()).isEqualTo(content);
    }
```

1. `@After` : Junit 에서 단위 테스트가 끝날 때마다 수행되는 메서드를 지정한다.
2. `postRepository.save` : posts 테이블에 **insert/update** 를 실행한다. id 값이 있다면 update, 있다면 insert를 실행한다.
3. `postRepository.findAll` : posts 테이블에 있는 전체 데이터를 조회한다. 

## 3.3 서비스 처리 방식

기존의 프로젝트에서 비지니스 로직을 서비스 클래스에서 처리를 했다. 

```java
대충 서비스 클래스 {
	public Order cancleOrder(int OrderId) {
		OrderDto order = orderDao.selectOrder(OrderId);
		String status = order.getStatus();
		if(!status.equals("progress"){
			order.setStatus("cancle");
			orderDao.update(order);
		}
		return order;
	}
}
```

서비스 계층이 무의미해지고 객체가 단순하게 데이터만 담는 역할만 하게 되어 `도메인 모델` 에서 처리하게된다.

```java
대충 서비스 클래스 {
	public Order cancleOrder(int OrderId) {
		OrderDto order = orderDao.selectOrder(OrderId);
		order.cancle();
		return order;
	}
}
```

order가 취소 이벤트를 처리하고 서비스는 트랜잭션과 도메인간의 순서만 보장해준다. 

## 3.4 등록/수정/조회 API 구성

### 1. DTO 클래스

```java
@Getter
@NoArgsConstructor
public class PostsSaveRequestDto {
    private String title;
    private String content;
    private String author;

    @Builder
    public PostsSaveRequestDto(String title, String content, String author){
        this.title = title;
        this.content = content;
        this.author = author;
    }
		
    public Posts toEntity(){
        return Posts.builder()
                .title(title)
                .content(content)
                .author(author)
                .build();
    }
}
```

이전에 생성했던 `Post` 클래스와 비슷한데 DTO 클래스를 따로 생성한 이유는 데이터베이스와 가장 가깝게 있는 클래스이기 때문이다. `Entity` 클래스를 기준으로 테이블이 생성되고 스키마가 변경되기 때문에 `Entity` 클래스를 변경하는 것은 아주 큰 변경이라 할 수 있다. Request와 Response용 DTO는 View를 위한 클래스이기 때문에 자주 변경되는데 Entitiy 클래스를 View와 DB를 위한 클래스로 같이 사용하는 것은 바람직하지 못하다.

`toEntity` 는 `Post` 객체를 바로 생성하지 않고 `toEntity` 라는 메서드를 통해 PostsSaveRequestDto를 `JPA` 와 매핑하기 위해 `엔티티화` 시켜준다.

> `**Getter, Setter` 를 사용한 객체 VS `Builder`을 사용한 객체**

- **Getter, Setter 을 사용한 객체**

```java
public class Student {
	private String name;
	private String stu_num;
	private String major;

	public void setName(String name){
		this.name = name;
	}
	public String getName(){
		return name;
	}
	public void setStu_num(String stu_num){
		this.stu_num= stu_num;
	}
	public String getStu_num(){
		return stu_num;
	}
	public void setMajor(String major){
		this.major= major;
	}
	public String getMajor()
		return major;
	}
}
```

만약 데이터베이스에 저장된 Student 테이블에서 name, stu_num, major 이 null을 허용하지 않는데 set을 하지않은 상태에서 get을 하게되면 nullpointerException이 발생하게된다.

- **Builder을 사용한 객체**

```java
public class Student {
	private String name; // not null
	private String stu_num; // not null
	private String major; // not null
	private String age; // option

	public Student(String name, String stu_num, String major, String age) {
		this.name = name;
		this.stu_num = stu_num;
		this.major = major;
		this.age = age;
	}

	static class Builder {
		private String name; // not null
		private String stu_num; // not null
		private String major; // not null
		private String age; // option

		public Builder withName(String name) {
			this.name = name;
			return this;
		}
		public Builder withStu_num(String stu_num) {
			this.stu_num = stu_num;
			return this;
		}
		public Builder withName(String major) {
			this.major = major;
			return this;
		}
		public Builder withName(String age) {
			this.age = age;
			return this;
		}
		public Student build() {
			if(name == null || stu_num == null || major == null) {
				throw new IllealStateException("Cannot create Student");
			}
			return new Student(name, stu_num, major, age);
		}
	}
}
```

### 2. Service 클래스

```java
@RequiredArgsConstructor
@Service
public class PostsService {
    private final PostsRepository postsRepository;

    public Long save(PostsSaveRequestDto requestDto){
        return postsRepository.save(requestDto.toEntity()).getId();
    }
}
```

스프링에서 빈 주입방식은 애너테이션을 이용한 방식, sette을 이용한 방식, 생성자로 주입받는 방식이 있다. `@RequiredArgsConstructor` 을 사용하면 된다. 

### 3. Controller

```java
@RequiredArgsConstructor
@RestController
public class PostsApiController {

    private final PostsService postsService;

    @PostMapping("/api/v1/posts")
    public Long save(@RequestBody PostsSaveRequestDto requestDto){
        return postsService.save(requestDto);
    }
}
```

/api/v1/posts 로 Post 요청이 오면 HTTP BODY 에 PostSaveRequestDto가 담겨서 온다. 

## 3.5 JPA Auditing으로 생성&수정시간 자동화

보통 엔티티에는 유지보수가 매우 중요하기 때문에 해당 데이터의 생성시간과 수정시간을 포함한다.