# item 3. private 생성자나 열거 타입으로 싱글턴임을 보증하라

## 3.1 싱글턴

자바 어플리케이션 내에서 인스턴스를 오직 하나만 생성할 수 있는 클래스를 `**싱글턴(Singleton)**` 패턴 클래스라고 한다. 하나만 생성하기 때문에 메모리를 아낄 수 있고 여러 객체에서 전역적으로 사용할 수 있다. 

> 싱글턴의 예

1. 함수와 같은 무상태 객체 : `무상태 객체` 는 필드가 존재하지 않는다. 존재하더라고 `final` 상수 이다. 
2. 설계상 유일해야하는 시스템 컴포넌트

> 싱글턴 단점

1. 생성자를 `private` 로 선언하기 때문에 상속할 수 없다
2. 테스트하기 어렵다. 

## 3.2 싱글턴 클래스 생성 방식

### 1. `public static 멤버가 final` 필드인 방식

```java
public class Manager {
		public static final Manager INSTANCE = new Manager();
		private Manager() { }
}
```

`Manager` 생성자는 `Manager.INSTANCE` 필드를 초기화 할 때 단 한번만 호출된다. `Manager.INSTANCE` 는 `public` 으로 선언되어있기 때문에 어플리케이션 전체에서 하나만 존재한다. 

예외가 있는데 `AccessibleObject.setAccessible` 을 사용한다면 `private` 생성자를 호출할 수 있다. 생성자를 수정하여 두번째 객체가 생성되면 예외를 던지도록 해야한다.

- 장점

    해당 클래스가 싱글턴임이 API 에 명백하게 드러난다.

### 2. 정적 팩토리 메서드를 이용한 방식

```java
private static final Manager INSTANCE = new Manager();
    private Manager(){
        // 객체가 생성되어있다면
        if(INSTANCE != null){
            throw new RuntimeException("이미 생성된 객체입니다");
        }
    }
    public static Manager getInstance() {
        return INSTANCE;
    }
}
```

### 3. 열거 타입 방식의 싱글턴

```java
public enum ManagerEnum {
    INSTANCE;
}
```