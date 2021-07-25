### 🐱‍🚀 String vs StringBuffer vs StringBuilder

​	`String` 의 가장 큰 특징은 `불변하다는 것` 이다.

```java
String str = new String("안녕하세요~");
str = str + "반갑습니다"; 
```

String 객체를 생성한 후 `+연산자` 로 문자열을 더해서 다시 str에 저장하면 기존에 생성된 String 객체의 값이 변경되는 것이 아닌 새로운 String 객체가 생성된다.  그리고 기존의 String 객체는 `Garbage collction` 에 의해 소멸된다. 

만약 자주 변경되지 않는 문자열이라면 조회하는데에 있어 `String`을 사용하는 것이 좋지만 문자열에 대한 연산이 많다면 `메모리의 Heap 영역` 에 많은 `Garbage`가 생성되어 성능에 영향을 미칠 수 있다고 한다.

`String의 불변성`을 해결하고자 `가변성`을 가지는 `StringBuffer, StringBuilder` 클래스를 사용하면 된다. 

#### 	🐱‍🐉 StringBuffer

​		멀티 스레드에 안전하게 설계되어 있고 여러개의 스레드에서 하나의 `StringBuffer` 객체에 접근해도 안전하다. (`StringBuffer` 클래스의 내부 함수들이 `synchronized` 되어있다고 함 / 아 그리고 String역시 불변성을 가져서 멀티스레드 환경에서 안전하다.) 

```java
StringBuffer sb = new StringBuffer("안녕하세요~");
sb.append("반갑습니다");
```

이렇게 하게 되면 StringBufer객체가 하나 더 생성되지 않고 문자열을 추가할 수 있다. 

문자열을 추가하는 것이 아닌 새로 초기화를 하고싶다면 다음과 같이 하면된다.

```java
StringBuffer sb = new StringBuffer("hello");
sb.setLengt(0); // 길이를 0으로 초기화한 후 
sb.append("Hello World");
```

`StringBuilder`역시 가변적이지만 `StringBuilder` 같이 멀티스레드 환경 사용하기에는 부적합하다. 단일 스레드 환경에서는 `StringBuilder` 의 성능이 좋다고 한다. 



### 🐱‍🚀 참고자료

1. [String, StringBuffer, StringBuilder](https://ifuwanna.tistory.com/221)

2. [StringBuffer 객체 초기화하기](https://blog.outsider.ne.kr/265)