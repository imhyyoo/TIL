## 🐱‍🚀 Javascript

#### 😒 Javascript 이벤트 처리

##### 	1. HTML 코드 안에서 Javascript 이벤트 처리 (onclick 이용)

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src = "crossEvent.js">
</script>
<script type="text/javascript">
	function processA(){
		var result = document.getElementById("result");
		result.innerHTML = "<span>이벤트결과</span>";
	}
	
</script>
</head>
<body>
	<!-- 버튼에 이벤트 발생! -->
	<input type="button" value="버튼1" onclick="processA()">&nbsp;&nbsp;
	<input type="button" id="btn" value="버튼2">
	<div id="result">
	</div>
</body>
</html>
```

​	위의 코드는 버튼1을 클릭했을 때 이벤트 결과라는 문구가 출력되는 코드이다. `HTML 문서` 내에서 직접 `input` 에 이벤트를 넣었다. 

##### 	2. window.onload 를 이용하여 이벤트 처리

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
</script>
<script type="text/javascript">
	function processA(){
		var result = document.getElementById("result");
		result.innerHTML = "<span>이벤트결과</span>";
	}
	
	function processB(){
		alert.log("이벤트 결과2");
	}

	// html에 있는 내용을 로딩하고나서 function을 호출
	window.onload = function(){
		var btn = document.getElementById("btn");
		btn.onclick = processA;
		btn.onclick = processB;
	}
</script>
</head>
<body>
	<input type="button" value="버튼1" onclick="processA()">&nbsp;&nbsp;
	<input type="button" id="btn" value="버튼2">
	<div id="result">
	</div>
</body>
</html>
```

​	코드를 실행하고 버튼2를 클릭하면 `ProcessA` 와 `ProcessB` 가 실행될 것이라고 예상했지만 실행결과는 그렇지 못하다. `processA` 는 실행되지 않고 `processB` 만 실행되었다. 이를 해결하기 위해 사용하는 것이 이벤트 리스너이다.

##### 	3. addeventlistener 함수를 이용한 이벤트 처리

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
</script>
<script type="text/javascript">
	function doProcess(){
		var result = document.getElementById("result");
		result.innerHTML = "<span>이벤트결과</span>";
	}
	
	function doProcess2(){
		alert("이벤트 결과2");
	}
	
	window.onload = function(){
		var btn = document.getElementById("btn");
		// 버튼이라는 한개의 대상에게 두개 이상의 이벤트  주입이 가능하다. 
		btn.addEventListener('click', doProcess2, false);
		btn.addEventListener('click', doProcess, false);
	}
</script>
</head>
<body>
	<input type="button" value="버튼1" onclick="doProcess()">&nbsp;&nbsp;
	<input type="button" id="btn" value="버튼2">
	<div id="result">
	</div>
</body>
</html>
```

​	`addeventlistener` 표준함수를 이용해서 `이벤트 이름, 이벤트 핸들러, 캡쳐 유무` 를 넣어줄 수 있다. 여기서 `capture` 속성은 `true` 일 때 핸들러는 캡처링 단계에서 동작하고 `false`일 때 핸들러는 버블링 단계에서 동작한다. `addeventlistener` 표준함수가 적용되지 않는 브라우저가 있는데 적용되지 않는 브라우저를 위한 이벤트 처리도 생각해봐야한다.

##### 	4. 크로스 브라우징 처리

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src = "crossEvent.js">
</script>
<script type="text/javascript">
	function doProcess(){
		var result = document.getElementById("result");
		result.innerHTML = "<span>이벤트결과</span>";
	}
	
	function doProcess2(){
		alert("이벤트 결과2");
	}
	// html에 있는 내용을 로딩하고나서 function을 호출하겠다는 의미
	window.onload = function(){
		var btn = document.getElementById("btn");
		cross.Event.addListener(btn, 'click', doProcess2, false);
		cross.Event.addListener(btn, 'click', doProcess, false);
	}
</script>
</head>
<body>
	<input type="button" value="버튼1" onclick="doProcess()">&nbsp;&nbsp;
	<input type="button" id="btn" value="버튼2">
	<div id="result">
	</div>
</body>
</html>
```

```javascript
var cross = {};
cross.Event = {}; // cross 객체에 event라는 객체가 들어간다(동적 프로퍼티 추가)
//어느 브라우저에게 작동하도록 하고싶음
cross.Event.addListener = function(element, name, handler, capture){
	// capture가 있으면 capture 사용하고 아니라면 false
    capture = capture | false; 
	// 표준 브라우저라면 addeventListener가 있음 → 표준 브라우저 값만 가져도 참이라고 판단
	if(element.addEventListener){
		element.addEventListener(name, handler, capture);
	} else {
		// 구현 ie element 이벤트의 대상
		element.attachEvent('on'+name, handler);
	}
}
```

​	`크로스 브라우징` 처리를 통해 어떤 브라우저에서도 동일하게 이벤트가 적용될 수 있다. 

#### 😒 버블링과 캡쳐링

* **이벤트 버블링**

  `이벤트 발생 요소`부터 요소를 포함하는 `부모 요소`까지 올라가면서 이벤트를 검사하는 것을 이벤트 버블링이라고 한다. 이벤트 버블링에서 버블 속성의 이벤트 핸들러가 등록되어 있으면 수행된다.

* **이벤트 캡쳐링**

  이벤트가 발생한 요소를 포함하는 부모 HTML 로부터 이벤트 근원지인 자식 요소까지 검사하는 것을 캡쳐링이라고 한다. 이벤트 캡쳐링에서 캡쳐 속성의 이벤트 핸들러가 등록되어 있으면 수행된다.

  ```javascript
  <script type="text/javascript">
  	function aProcess(){
  		alert("aa");
  	}
  	
  	// e는 이벤트 객체가 된다. 
  	function bProcess(e){
  		alert("bb");
  		var event = window.event || e; // 구형 브라우저에서 윈도우 객체 : window.event
  		cross.Event.stopBubble(event); // 이벤트 전달을 멈춘다
  	}
  	
  	window.onload = function(){
  		var aa = document.getElementById("aa");
  		var bb = document.getElementById("bb");
  		aa.onclick = aProcess;
  		bb.onclick = bProcess;
  	}
  </script>
  </head>
  <body>
  <!-- 이벤트 버블링 -->
  	<div id="aa">
  		aaaaaaaaaaa
  		<div id="bb">
  			bbbbbbbbb
  		</div>
  		aaaaaaaaaaa
  	</div>
  </body>
  
  // JS 파일
  cross.Event.stopBubble = function(event){
  	//표준 브라우저일 때
  	if(event.stopPropagation){
  		event.stopPropagation(); // 자식에서 부모로 이벤트가 흘러가는 것을 방지시킬 수 있다.
  		
  	} else {
  		event.cancelBubble = true;
  	}
  }
  ```

#### 😒 이벤트 핸들러 함수

​	이벤트가 발생했을 때 호출되는 함수를 이벤트 핸들러 함수라고 한다. 

#### 😒 JavaScript 이벤트 종류

##### 1. onclick

​	클릭 이벤트가 발생했을 때 정해놓은 코드가 실행되도록 한다.

##### 2. onload / onunload

​	사용자가 웹 페이지에 진입하거나 웹 페이지를 떠날 때 onload와 onunload 발생.

##### 3. onchange 

​	해당 요소에 변화가 발생했을 때 정해놓은 코드가 실행되도록 한다.

##### 4. onmouseover

​	사용자가 마우스를 요소 위에 올리면 이벤트가 발생한다. 

#### 😒 this 

* `일반함수에서 this`는 `윈도우 객체` 다.
* `윈도우 객체`는 `전역변수`를 의미한다.
* `생성자 함수나 프로토타입 메서드`에서 this는 자기자신객체를가리킨다.
* `이벤트 핸들러 함수` 내에서 `this`는 이벤트가 발생한 대상이다.

#### 😒 이것저것.. 

* `window.open('오픈할 문서이름', '주소창에 띄울 문구', '크기') ` : 자식 윈도우 오픈
* `window.opener` : opener을 이용하면 부모 window에 접근할 수 있다. 
* `innerText` : 요소 안의 텍스트 값만 가져온다. 
* `innerHTML` : element 안의 html이나 xml을 가져온다.

