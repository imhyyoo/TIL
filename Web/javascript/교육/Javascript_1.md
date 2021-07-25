##  🐱‍🚀 Javascript

#### 😒 자바스크립트 이해 

​	자바스크립트는 객체를 기반한 프론트엔드 개발 언어고 정적인 HTML 문서에 동작을 주기위해 사용한다. 예를들어 사이트 메뉴에 마우스를 올렸을 때 메뉴가 펼쳐지는 행동을 Javascript와 JQuery를 사용하여 표현한다. 자바스크립트 특징은 다음과 같이 정리해볼 수 있다.

* 플랫폼에 독립적이고
* 비동기식으로 구현(여기서 말하는 비동기식이란 요청과 결과가 동시에 일어나지 않는다는 뜻이다)
* 인터프리터 언어로 빠르게 개발할 수 있다. (인터프리터 언어란 모든 소스코드를 기계어로 한번에 만들어서 실행하는 것이 아닌 코드 한줄을 바로 기계어로 변환하여 실행한다)
* HTML `< head >` 나 `< body >` 부분에 위치한다. 

#### 😒 Javascript의 위치

* **html 내부에 위치**

  ```javascript
  <!DOCTYPE html>
  <html>
  <head>
      <meta charset="UTF-8">
      <title>Insert title here</title>
      <script type="text/javascript">
         alert('안녕하세요');
      </script>
  </head>
  <body>
  </body>
  </html>
  ```

* **외부 스크립트 **

  ```javascript
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="UTF-8">
  <title>Insert title here</title>
  <script type="text/javascript" src="myjs.js"></script>
  </head>
  ```

#### 😒 변수 선언

​	자바스크립트에서 변수에 저장할 수 있는 자료형은 `문자형(String)` , `숫자형(Number)`, `논리형(Boolean)`, `빈데이터(Undefined)`  

* **typeof** : 변수에 저장된 자료형을 알아내는데 사용된다.

* **null 이 저장된 변수** : null은 데이터 타입이 object 다.

* 자바스크립트는 함수를 변수에 넣을 수 있다. 

* **문자열을 정수형으로 바꿔주는 함수** : `eval( )` , `parseInt( )`, `Number( )`

* 자바스크립트는 데이터 타입이 다르면 알아서 변환해준다

  ```javascript
  var num = "10";
  if(num == 10){
      alert('안녕하세요');
  } else {
      alert('다르다');
  }
  ```

#### 😒 var / let / const

* **VAR**

  ES6 이전의 변수 선언 방식이다. `var` 의 단점은 `함수 단위의 영역`을 가진다.

  ```javascript
  <script type="text/javascript">
  	var num = 30;
  	function test(){
  		var num = 40;
  		console.log(num);
  	}
  	test();
  	console.log(num);
  </script>
  ```

  위의 결과는 40 과 30 순으로 나온다.  `num = 40` 은 테스트 함수에서만 존재하고 함수 실행이 끝나면 사라지게 된다. 그리고 var은 변수의 재선언이 가능하다.

  ```javascript
  var num = 30;
  var num = 40;
  ```

* **let**

  let을 사용해서 변수를 정의하고나서 변수 재선언 불가능하고 변수의 재 초기화는 가능하다. 

  ```javascript
  <script type="text/javascript">
  	let num = 'hello';
  	num = 'hello2'; // 가능
  	let num = 'hello3' // 불가능
  </script>
  ```

* **const**

  const를 사용하면 변수 재선언과 재할당 전부 불가능하다.

  ```javascript
  const num = 50;
  const num = 40; // 불가능
  num = 30; // 불가능
  ```

#### 😒 함수

* **선언적 함수**

  선언적 함수는 메모리에 미리 로드된다. 미리 로드되어있기 때문에 함수가 뒤에 선언되어있어도 메모리에 올라와져 있어서 어디서든 사용할 수 있다. 

  ```javascript
  function 함수명(){
      자바스크립트 코드;
  } 
  ```

* **익명 함수**

  익명 함수는 메모리에 미리 로드되지 않고 인터프리터 형식때문에 메모리에 순차적으로 올라가게된다. 익명함수를 사용하려면 익명함수가 정의된 곳 뒤에서 호출해야한다. 

  ```javascript
  var func = function() {
      // 함수 내용
  }
  ```

* **함수 특징**

  * 자바스크립트에서는 변수에 함수를 전달할 수 있다.

    ```javascript
    var add = function() {
        // 함수 내용
    }
    var copy = add; // copy는 함수를 저장하는 변수
    var copy2 = add(); // copy2는 add 함수의 결과 값을 저장하는 변수
    ```

  * 자바스크립트에서는 함수의 매개변수에 함수를 전달할 수 있다

    ```javascript
    var func = function(func) {
        if(typeof func == 'function'){
            func();
        }
    }
    
    func(function(){
        console.log('함수 호출');
    })
    ```

* **자바스크립트 내장함수**
  - **encodeURI()** : 문자를 유니코드 값으로 인코딩한다.
  - **decodeURI()** : 유니코드 값을 디코딩해 다시 문자화한다.
  - **parseInt()** : 문자열 데이터를 실수형 데이터로 반환한다.
  - **String()** : 문자형 데이터로 반환한다.
  - **isNaN()** : Is not a null 의 약자로 숫자가 아닌 문자가 포함되어 있으면 true 리턴한다. 

#### 😒 클로저

​	클로저는 내부 함수를 리턴해서 실행 컨텍스트에서 사용할 수 없는 변수들을 계속 사용할 수 있게  해주는 것이다. 내부에 있는 함수가 자신이 속한 외부 함수의 지역 변수에 접근할 수 있는데 외부 함수의 실행이 끝나도 내부 함수를 리턴하게된다면 자신이 속한 외부 함수의 변수에 접근할 수 있다. 

```javascript
function outerFunc(){
    var num = 1;
    var innerFunc = function() {
        console.log(num)
    }
    return innerFunc;
}

var inner = outerFunc();
inner();
```

```javascript
function outer(arg1, arg2){
    var local = 10;
    function inner(innerarg) {
        console.log((arg1 + arg2)/innerarg + local);
    }
    return inner;
}

var result = outer(10, 20);
result(20);
```

outer(10, 20) 의 결과 값이 result에 저장된다. outer 의 리턴값은 `function inner(innerarg) { console.log((10 + 20)/innerarg + 10)}` 다. 

result(20) 을 넣게되면 inner(20) { console.log((10, 20)/20+10)} 이 호출된다. outer 함수가 끝났지만 inner 함수가 남아있기 때문에 접근이 가능한 것이다. 

#### 😒 객체

​	자바스크립트 객체에는 `내장 객체`, `브라우저 객체 모델(BOM, Browser Object Model)`, `문서 객체 모델(DOM, Document Object Model)` 로 나눌 수 있다. 

* **내장 객체**

  자바스크립트 엔진에 내장되어 있어 필요한 경우 생성해서 사용한다. 문자, 날짜, 배열 수학 등이 있다.

* **브라우저 객체 모델**

  브라우저에 계층 구조로 내장되어 있는 객체를 말한다. 브라우저 객체로는 `window`, `screen`, `location`, `history`, `navigator` 객체 등이 있다. 

  `window` 는 `document`와 `location` 객체의 상위 객체이다. 현재 열려있는 사이트에서 다른 사이트로 이동하려면 `location` 객체의 `href` 주소를 바꾸면 된다. 

  `window`는 브라우저, `location` 은 주소 창, `document`는 문서 영역이다.

* **문서 객체 모델**

  문서 객체 모델(DOM)은 `HTML 문서 구조` 를 뜻한다. 최상위 객체로는 `html` 하위 객체로는 `head`, `body` 가 있다. 

> **객체 생성 방법**

​	자바스크립트에서 객체를 생성하는 방법은 다음과 같다.

##### 1. 기존 Object 객체 사용

​	기존 Object 객체를 사용하여 `동적으로 객체 속성을 추가` 가 가능하다. 객체 생성 이후 수정이 가능하다.

```javascript
var obj = new Object();
```

