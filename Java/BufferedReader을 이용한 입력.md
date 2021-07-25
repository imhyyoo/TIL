### 🐱‍🚀 BufferedReader을 이용한 입력

​	Scanner로 입력받는다고 해보자. 정수 10개를 입력받으려면 `scanner 객체의 nextInt()` 를 반복문을 통해 10번 호출해야한다. 입력 갯수가 크면 그만큼 반복문도 많이 돌아서 시간이 많이 걸린다. 

​	우리는 입력을 받을 때 `System.in` 을 통해 받게된다. in은 `InputStream` 타입의 멤버변수였다. `InputStream`은 입력 스트림으로부터 1바이트를 읽어온다. 한바이트씩 읽게되면 2바이트인 한글을 입력하면 문제가 생긴다. `InputStreamReader`은 한바이트 단위로 읽어오는 형식을 문자단위로 변환시켜준다고 한다. 

​	그러면 `Scanner` 객체에서 InputStream 타입의 in 을 통해 입력을 받는데 한바이트만 받는거 아니냐라고 생각을 했는데 `Scanner 생성자`를 한번 살펴보았다. 

```java
public Scanner(InputStream source) {
        this(new InputStreamReader(source), WHITESPACE_PATTERN);
}
```

​	Scanner (System.in (InputStream 타입) ) 생성자를 호출하면 내부적으로 this(InputStreamReader, ~) 생성자를 호출하게 된다. 그래서 한바이트만 읽는 것이 아닌 문자단위로 읽어올 수 있다. 

​	다시 돌아와서.. 한 바이트씩 읽어오는 `InputStream`을 `InputStreamReader` 을 통해 더 효율적으로 입력받아올 수 있다. 

```java
	InputStreamReader isr = new InputStreamReader(System.in);
	char[] c = new char[10];
	sr.read(c);
	for(char str : c) {
        System.out.println(str);
    }
```

​	InputStreamReader은 문자를 처리한다. `BufferedReader`을 통해 문자들을 쌓아둔 후 한번에 문자열로 처리한다. 

```java
	BufferedReader br = new BufferedReader(
    	new InputStreamReader(System.in)
    );
```

​	`BufferedReader`은 바이트 타입으로 읽어들이는 in을 char로 처리한 뒤 String으로 저장할 수 있다고 한다. 버퍼가 있는 스트림이고 Scanner처럼 정규식을 검사하지 않기 때문에 `Scanner에 비해 우수하다` . BufferedReader로 입력을 받을 때 다음과 같이 받는다. 

```java
	BufferReader br = new BufferedReader(new InputStreamReader(System.in));
	try {
        String str = br.readLine();
    } catch(Exception e) {
        e.printStackTrace();
    }
```

### 🐱‍🚀 참고자료

​	1. [JAVA 입력 뜯어보기](https://st-lab.tistory.com/41)