# item 4. 인스턴스화를 막으려거든 private 생성자를 사용하라

기본 생성자에 `private` 키워드를 붙이면 클래스의 인스턴스화를 막을 수 있다.

```java
public class Manager {
		private Manager() {
			throw new AssertionError();
		}
}
```

`AssertionError` 은 실수로 생성자가 호출되는 것을 막아준다.