### π±βπ String vs StringBuffer vs StringBuilder

β	`String` μ κ°μ₯ ν° νΉμ§μ `λΆλ³νλ€λ κ²` μ΄λ€.

```java
String str = new String("μλνμΈμ~");
str = str + "λ°κ°μ΅λλ€"; 
```

String κ°μ²΄λ₯Ό μμ±ν ν `+μ°μ°μ` λ‘ λ¬Έμμ΄μ λν΄μ λ€μ strμ μ μ₯νλ©΄ κΈ°μ‘΄μ μμ±λ String κ°μ²΄μ κ°μ΄ λ³κ²½λλ κ²μ΄ μλ μλ‘μ΄ String κ°μ²΄κ° μμ±λλ€.  κ·Έλ¦¬κ³  κΈ°μ‘΄μ String κ°μ²΄λ `Garbage collction` μ μν΄ μλ©Έλλ€. 

λ§μ½ μμ£Ό λ³κ²½λμ§ μλ λ¬Έμμ΄μ΄λΌλ©΄ μ‘°ννλλ°μ μμ΄ `String`μ μ¬μ©νλ κ²μ΄ μ’μ§λ§ λ¬Έμμ΄μ λν μ°μ°μ΄ λ§λ€λ©΄ `λ©λͺ¨λ¦¬μ Heap μμ­` μ λ§μ `Garbage`κ° μμ±λμ΄ μ±λ₯μ μν₯μ λ―ΈμΉ  μ μλ€κ³  νλ€.

`Stringμ λΆλ³μ±`μ ν΄κ²°νκ³ μ `κ°λ³μ±`μ κ°μ§λ `StringBuffer, StringBuilder` ν΄λμ€λ₯Ό μ¬μ©νλ©΄ λλ€. 

#### 	π±βπ StringBuffer

β		λ©ν° μ€λ λμ μμ νκ² μ€κ³λμ΄ μκ³  μ¬λ¬κ°μ μ€λ λμμ νλμ `StringBuffer` κ°μ²΄μ μ κ·Όν΄λ μμ νλ€. (`StringBuffer` ν΄λμ€μ λ΄λΆ ν¨μλ€μ΄ `synchronized` λμ΄μλ€κ³  ν¨ / μ κ·Έλ¦¬κ³  Stringμ­μ λΆλ³μ±μ κ°μ Έμ λ©ν°μ€λ λ νκ²½μμ μμ νλ€.) 

```java
StringBuffer sb = new StringBuffer("μλνμΈμ~");
sb.append("λ°κ°μ΅λλ€");
```

μ΄λ κ² νκ² λλ©΄ StringBuferκ°μ²΄κ° νλ λ μμ±λμ§ μκ³  λ¬Έμμ΄μ μΆκ°ν  μ μλ€. 

λ¬Έμμ΄μ μΆκ°νλ κ²μ΄ μλ μλ‘ μ΄κΈ°νλ₯Ό νκ³ μΆλ€λ©΄ λ€μκ³Ό κ°μ΄ νλ©΄λλ€.

```java
StringBuffer sb = new StringBuffer("hello");
sb.setLengt(0); // κΈΈμ΄λ₯Ό 0μΌλ‘ μ΄κΈ°νν ν 
sb.append("Hello World");
```

`StringBuilder`μ­μ κ°λ³μ μ΄μ§λ§ `StringBuilder` κ°μ΄ λ©ν°μ€λ λ νκ²½ μ¬μ©νκΈ°μλ λΆμ ν©νλ€. λ¨μΌ μ€λ λ νκ²½μμλ `StringBuilder` μ μ±λ₯μ΄ μ’λ€κ³  νλ€. 



### π±βπ μ°Έκ³ μλ£

1. [String, StringBuffer, StringBuilder](https://ifuwanna.tistory.com/221)

2. [StringBuffer κ°μ²΄ μ΄κΈ°ννκΈ°](https://blog.outsider.ne.kr/265)