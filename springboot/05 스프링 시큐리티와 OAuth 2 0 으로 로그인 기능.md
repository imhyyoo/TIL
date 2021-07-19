# 05 스프링 시큐리티와 OAuth 2.0 으로 로그인 기능 구현하기

스프링 시큐리티는 `인증` 과 `인가` 기능을 가지고있는 프레임워크이다. `인터셉터, 필터 기반` 의 보안 기능 구현보다 `스프링 시큐리티` 를 통한 보안 기능 구현을 권장한다. 

## 5.1 스프링 시큐리티와 스프링 시큐리티 OAuth2 클라이언트

## 5.2 구글 서비스 등록

구글 로그인을 사용하기 위해서는 서비스 등록이 필요.

## 5.3 로그인 구현을 위한 클래스 생성

### 1. User 엔티티

사용자 정보와 테이블을 매핑하는 엔티티 클래스를 생성해야한다.

```java
@Getter
@NoArgsConstructor
@Entity
public class User extends BaseTimeEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @Column(nullable = false)
    private String email;

    @Column
    private String picture;

    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private Role role;

    @Builder
    public User(String name, String email, String picture, Role role) {
        this.name = name;
        this.email = email;
        this.picture = picture;
        this.role = role;
    }

    public User update(String name, String picture){
        this.name = name;
        this.picture = picture;
        return this;
    }

    public String getRoleKey(){
        return this.role.getKey();
    }
}
```

1. `**Enumerated(EnumType.STRING)**` : JPA로 데이터베이스에 저장할 때 Enum 값을 어떤 형태로 저장할지 결정한다. 기본적으로는 `int` 로 된 숫자가 저장된다. 

### 2. Role 클래스

사용자의 권한을 관리해주는 `Enum` 클래스를 생성해야한다.

```java
@Getter
@RequiredArgsConstructor
public enum Role {
    GUEST("ROLE_GUEST", "손님"),
    USER("ROLE_USER", "일반 사용자");
    private final String key;
    private final String title;
}
```

1. 스프링 시큐리티에서는 권한명에 `ROLE_` 가 붙어야한다.

### 3. UserRepository 클래스

User의 CRUD를 담당하는 클래스다.

```java
public interface UserRepository extends JpaRepository<User, Long> {
    // 반환되는 값 중 이메일을 통해 이미 생성된 사용자인지 확인
    Optional<User> findByEmail(String email);
}
```

## 5.4 스프링 시큐리티 설정

### 1. 스프링 시큐리티 의존성 추가

`**spring-boot-starter-oauth2-client`** 

### 2. 시큐티리 설정파일 추가

```java
@RequiredArgsConstructor
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    private final CustomOAuth2UserService customOAuth2UserService;

    protected void configure(HttpSecurity http) throws Exception{
        http.csrf().disable().headers().frameOptions().disable()
                .and()
                .authorizeRequests()
                .antMatchers("/", "/css/**", "/images/**", "/js/**", "/h2-console/**", "/profile").permitAll()
                .antMatchers("/api/v1/**").hasRole(Role.USER.name())
                .anyRequest().authenticated()
                .and()
                .logout().logoutSuccessUrl("/")
                .and()
                .oauth2Login()
                .userInfoEndpoint().userService(customOAuth2UserService);

    }
}
```

1. `@EnableWebSecurity` : 스프링 시큐리티 설정을 활성화시킨다
2. `csrf().disable().headers().frameOptions().disable()` : h2 console 화면을 사용하기 위해 해당 옵션을 disable 한다.
3. `authorizeRequests` : URL별 권한 관리를 설정하는 옵션의 시작점
4. `antMatchers` : 3번이 선언되어야 사용할 수 있고 권한 관리 대상을 지정. URL, HTTP 메서드 별로 관리 가능. URL, HTTP 메서드 별로 관리 가능. 
5. `anyRequest` : 설정된 값 이외 나머지 URL 들을 나타낸다. 
6. `logout().logoutSuccessUrl("/")` : 로그아웃성공시 / 로 이동
7. `userInfoEndpoint()` : 로그인 성공 후 사용자 정보를 가져올 때 설정 담당
8. `userService()` : 로그인 성공 시 후속조치를 진행할 `UserService` 인터페이스의 구현체를 등록. 

> **SessionUser 클래스**

```java
@Getter
public class SessionUser implements Serializable {
    private String name;
    private String email;
    private String picture;

    public SessionUser(User user){
        this.name = user.getName();
        this.email = user.getEmail();
        this.picture = user.getPicture();
    }
}
```

SessionUser 객체에는 인증된 사용자 정보만 필요하다. User 객체를 세션에 저장하면 되는데 User을 사용하지 않고 별도로 만들어준 이유는 `User` 클래스는 엔티티이기 때문이다. 엔티티는 언제든 다른 엔티티와 관계를 맺을 수 있기 때문에 엔티티 클래스를 직렬화해선 안된다. 

> **CustomOAuth2UserService 클래스**

```java
@RequiredArgsConstructor
@Service
public class CustomOAuth2UserService implements OAuth2UserService<OAuth2UserRequest, OAuth2User> {
    private final UserRepository userRepository;
    private final HttpSession httpSession;

    @Override
    public OAuth2User loadUser(OAuth2UserRequest userRequest) throws OAuth2AuthenticationException {
        
				OAuth2UserService<OAuth2UserRequest, OAuth2User> delegate = new DefaultOAuth2UserService();
        OAuth2User oAuth2User = delegate.loadUser(userRequest);

        String registrationId = userRequest.getClientRegistration().getProviderDetails()
                .getUserInfoEndpoint().getUserNameAttributeName();

        String userNameAttributeName = userRequest.getClientRegistration().getClientId();

        OAuthAttributes attributes = OAuthAttributes.of(registrationId, userNameAttributeName,
                oAuth2User.getAttributes());

        User user = saveOrUpdate(attributes);
        httpSession.setAttribute("user", new SessionUser(user));

        return new DefaultOAuth2User(
                Collections.singleton(new SimpleGrantedAuthority(user.getRoleKey())),
                attributes.getAttributes(),
                attributes.getNameAttributeKey());
    }

    private User saveOrUpdate(OAuthAttributes attributes) {
        User user = userRepository.findByEmail(attributes.getEmail())
                .map(entity -> entity.update(attributes.getName(), attributes.getPicture()))
                .orElse(attributes.toEntity());

        return userRepository.save(user);
    }
}
```

`CustomOAuth2UserService`는 구글 로그인 이후 가져온 사용자의 정보를 기반으로 가입 및 정보수정, 세션 저장 기능을 한다. 

1. `OAuth2UserService` : 소셜 로그인을 구현하기 위해 `UserService` 가 아닌 `OAuthUserService` 를 상속받아야한다
2. `registerationId` : 현재 로그인 진행중인 서비스를 구분하는 변수
3. `userNameAttributeName` : 로그인 진행 시 키가 되는 필드 값. 네이버와 카카오 로그인은 지원하지 않고 구글의 기본 값은 `sub` 이다.
4. `OAuthAttributes` : OAuth2UserService를 통해 가져온 OAuth2User의 어트리뷰트를 담을 클래스

> **OAuthAttributes 클래스**

```java
@Getter
public class OAuthAttributes {
    private Map<String, Object> attributes;
    private String nameAttributeKey;
    private String name;
    private String email;
    private String picture;

    @Builder
    public OAuthAttributes(Map<String, Object> attributes, String nameAttributeKey, String name, 
                           String email, String picture) {
        this.attributes = attributes;
        this.nameAttributeKey = nameAttributeKey;
        this.name = name;
        this.email = email;
        this.picture = picture;
    }

    public static OAuthAttributes of(String registrationId, String userNameAttributeName, 
                                     Map<String, Object> attributes) {
        return ofGoogle(userNameAttributeName, attributes);
    }

    private static OAuthAttributes ofGoogle(String userNameAttributeName, Map<String, Object> attributes) {
        return OAuthAttributes.builder()
                .name((String) attributes.get("name"))
                .email((String) attributes.get("email"))
                .picture((String) attributes.get("picture"))
                .attributes(attributes)
                .nameAttributeKey(userNameAttributeName)
                .build();
    }

    public User toEntity() {
        return User.builder()
                .name(name)
                .email(email)
                .picture(picture)
                .role(Role.GUEST)
                .build();
    }
}
```