## ๐ฑโ๐ Javascript

#### ๐ Javascript ์ด๋ฒคํธ ์ฒ๋ฆฌ

##### 	1. HTML ์ฝ๋ ์์์ Javascript ์ด๋ฒคํธ ์ฒ๋ฆฌ (onclick ์ด์ฉ)

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
		result.innerHTML = "<span>์ด๋ฒคํธ๊ฒฐ๊ณผ</span>";
	}
	
</script>
</head>
<body>
	<!-- ๋ฒํผ์ ์ด๋ฒคํธ ๋ฐ์! -->
	<input type="button" value="๋ฒํผ1" onclick="processA()">&nbsp;&nbsp;
	<input type="button" id="btn" value="๋ฒํผ2">
	<div id="result">
	</div>
</body>
</html>
```

โ	์์ ์ฝ๋๋ ๋ฒํผ1์ ํด๋ฆญํ์ ๋ ์ด๋ฒคํธ ๊ฒฐ๊ณผ๋ผ๋ ๋ฌธ๊ตฌ๊ฐ ์ถ๋ ฅ๋๋ ์ฝ๋์ด๋ค. `HTML ๋ฌธ์` ๋ด์์ ์ง์  `input` ์ ์ด๋ฒคํธ๋ฅผ ๋ฃ์๋ค. 

##### 	2. window.onload ๋ฅผ ์ด์ฉํ์ฌ ์ด๋ฒคํธ ์ฒ๋ฆฌ

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
		result.innerHTML = "<span>์ด๋ฒคํธ๊ฒฐ๊ณผ</span>";
	}
	
	function processB(){
		alert.log("์ด๋ฒคํธ ๊ฒฐ๊ณผ2");
	}

	// html์ ์๋ ๋ด์ฉ์ ๋ก๋ฉํ๊ณ ๋์ function์ ํธ์ถ
	window.onload = function(){
		var btn = document.getElementById("btn");
		btn.onclick = processA;
		btn.onclick = processB;
	}
</script>
</head>
<body>
	<input type="button" value="๋ฒํผ1" onclick="processA()">&nbsp;&nbsp;
	<input type="button" id="btn" value="๋ฒํผ2">
	<div id="result">
	</div>
</body>
</html>
```

โ	์ฝ๋๋ฅผ ์คํํ๊ณ  ๋ฒํผ2๋ฅผ ํด๋ฆญํ๋ฉด `ProcessA` ์ `ProcessB` ๊ฐ ์คํ๋  ๊ฒ์ด๋ผ๊ณ  ์์ํ์ง๋ง ์คํ๊ฒฐ๊ณผ๋ ๊ทธ๋ ์ง ๋ชปํ๋ค. `processA` ๋ ์คํ๋์ง ์๊ณ  `processB` ๋ง ์คํ๋์๋ค. ์ด๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด ์ฌ์ฉํ๋ ๊ฒ์ด ์ด๋ฒคํธ ๋ฆฌ์ค๋์ด๋ค.

##### 	3. addeventlistener ํจ์๋ฅผ ์ด์ฉํ ์ด๋ฒคํธ ์ฒ๋ฆฌ

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
		result.innerHTML = "<span>์ด๋ฒคํธ๊ฒฐ๊ณผ</span>";
	}
	
	function doProcess2(){
		alert("์ด๋ฒคํธ ๊ฒฐ๊ณผ2");
	}
	
	window.onload = function(){
		var btn = document.getElementById("btn");
		// ๋ฒํผ์ด๋ผ๋ ํ๊ฐ์ ๋์์๊ฒ ๋๊ฐ ์ด์์ ์ด๋ฒคํธ  ์ฃผ์์ด ๊ฐ๋ฅํ๋ค. 
		btn.addEventListener('click', doProcess2, false);
		btn.addEventListener('click', doProcess, false);
	}
</script>
</head>
<body>
	<input type="button" value="๋ฒํผ1" onclick="doProcess()">&nbsp;&nbsp;
	<input type="button" id="btn" value="๋ฒํผ2">
	<div id="result">
	</div>
</body>
</html>
```

โ	`addeventlistener` ํ์คํจ์๋ฅผ ์ด์ฉํด์ `์ด๋ฒคํธ ์ด๋ฆ, ์ด๋ฒคํธ ํธ๋ค๋ฌ, ์บก์ณ ์ ๋ฌด` ๋ฅผ ๋ฃ์ด์ค ์ ์๋ค. ์ฌ๊ธฐ์ `capture` ์์ฑ์ `true` ์ผ ๋ ํธ๋ค๋ฌ๋ ์บก์ฒ๋ง ๋จ๊ณ์์ ๋์ํ๊ณ  `false`์ผ ๋ ํธ๋ค๋ฌ๋ ๋ฒ๋ธ๋ง ๋จ๊ณ์์ ๋์ํ๋ค. `addeventlistener` ํ์คํจ์๊ฐ ์ ์ฉ๋์ง ์๋ ๋ธ๋ผ์ฐ์ ๊ฐ ์๋๋ฐ ์ ์ฉ๋์ง ์๋ ๋ธ๋ผ์ฐ์ ๋ฅผ ์ํ ์ด๋ฒคํธ ์ฒ๋ฆฌ๋ ์๊ฐํด๋ด์ผํ๋ค.

##### 	4. ํฌ๋ก์ค ๋ธ๋ผ์ฐ์ง ์ฒ๋ฆฌ

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
		result.innerHTML = "<span>์ด๋ฒคํธ๊ฒฐ๊ณผ</span>";
	}
	
	function doProcess2(){
		alert("์ด๋ฒคํธ ๊ฒฐ๊ณผ2");
	}
	// html์ ์๋ ๋ด์ฉ์ ๋ก๋ฉํ๊ณ ๋์ function์ ํธ์ถํ๊ฒ ๋ค๋ ์๋ฏธ
	window.onload = function(){
		var btn = document.getElementById("btn");
		cross.Event.addListener(btn, 'click', doProcess2, false);
		cross.Event.addListener(btn, 'click', doProcess, false);
	}
</script>
</head>
<body>
	<input type="button" value="๋ฒํผ1" onclick="doProcess()">&nbsp;&nbsp;
	<input type="button" id="btn" value="๋ฒํผ2">
	<div id="result">
	</div>
</body>
</html>
```

```javascript
var cross = {};
cross.Event = {}; // cross ๊ฐ์ฒด์ event๋ผ๋ ๊ฐ์ฒด๊ฐ ๋ค์ด๊ฐ๋ค(๋์  ํ๋กํผํฐ ์ถ๊ฐ)
//์ด๋ ๋ธ๋ผ์ฐ์ ์๊ฒ ์๋ํ๋๋ก ํ๊ณ ์ถ์
cross.Event.addListener = function(element, name, handler, capture){
	// capture๊ฐ ์์ผ๋ฉด capture ์ฌ์ฉํ๊ณ  ์๋๋ผ๋ฉด false
    capture = capture | false; 
	// ํ์ค ๋ธ๋ผ์ฐ์ ๋ผ๋ฉด addeventListener๊ฐ ์์ โ ํ์ค ๋ธ๋ผ์ฐ์  ๊ฐ๋ง ๊ฐ์ ธ๋ ์ฐธ์ด๋ผ๊ณ  ํ๋จ
	if(element.addEventListener){
		element.addEventListener(name, handler, capture);
	} else {
		// ๊ตฌํ ie element ์ด๋ฒคํธ์ ๋์
		element.attachEvent('on'+name, handler);
	}
}
```

โ	`ํฌ๋ก์ค ๋ธ๋ผ์ฐ์ง` ์ฒ๋ฆฌ๋ฅผ ํตํด ์ด๋ค ๋ธ๋ผ์ฐ์ ์์๋ ๋์ผํ๊ฒ ์ด๋ฒคํธ๊ฐ ์ ์ฉ๋  ์ ์๋ค. 

#### ๐ ๋ฒ๋ธ๋ง๊ณผ ์บก์ณ๋ง

* **์ด๋ฒคํธ ๋ฒ๋ธ๋ง**

  `์ด๋ฒคํธ ๋ฐ์ ์์`๋ถํฐ ์์๋ฅผ ํฌํจํ๋ `๋ถ๋ชจ ์์`๊น์ง ์ฌ๋ผ๊ฐ๋ฉด์ ์ด๋ฒคํธ๋ฅผ ๊ฒ์ฌํ๋ ๊ฒ์ ์ด๋ฒคํธ ๋ฒ๋ธ๋ง์ด๋ผ๊ณ  ํ๋ค. ์ด๋ฒคํธ ๋ฒ๋ธ๋ง์์ ๋ฒ๋ธ ์์ฑ์ ์ด๋ฒคํธ ํธ๋ค๋ฌ๊ฐ ๋ฑ๋ก๋์ด ์์ผ๋ฉด ์ํ๋๋ค.

* **์ด๋ฒคํธ ์บก์ณ๋ง**

  ์ด๋ฒคํธ๊ฐ ๋ฐ์ํ ์์๋ฅผ ํฌํจํ๋ ๋ถ๋ชจ HTML ๋ก๋ถํฐ ์ด๋ฒคํธ ๊ทผ์์ง์ธ ์์ ์์๊น์ง ๊ฒ์ฌํ๋ ๊ฒ์ ์บก์ณ๋ง์ด๋ผ๊ณ  ํ๋ค. ์ด๋ฒคํธ ์บก์ณ๋ง์์ ์บก์ณ ์์ฑ์ ์ด๋ฒคํธ ํธ๋ค๋ฌ๊ฐ ๋ฑ๋ก๋์ด ์์ผ๋ฉด ์ํ๋๋ค.

  ```javascript
  <script type="text/javascript">
  	function aProcess(){
  		alert("aa");
  	}
  	
  	// e๋ ์ด๋ฒคํธ ๊ฐ์ฒด๊ฐ ๋๋ค. 
  	function bProcess(e){
  		alert("bb");
  		var event = window.event || e; // ๊ตฌํ ๋ธ๋ผ์ฐ์ ์์ ์๋์ฐ ๊ฐ์ฒด : window.event
  		cross.Event.stopBubble(event); // ์ด๋ฒคํธ ์ ๋ฌ์ ๋ฉ์ถ๋ค
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
  <!-- ์ด๋ฒคํธ ๋ฒ๋ธ๋ง -->
  	<div id="aa">
  		aaaaaaaaaaa
  		<div id="bb">
  			bbbbbbbbb
  		</div>
  		aaaaaaaaaaa
  	</div>
  </body>
  
  // JS ํ์ผ
  cross.Event.stopBubble = function(event){
  	//ํ์ค ๋ธ๋ผ์ฐ์ ์ผ ๋
  	if(event.stopPropagation){
  		event.stopPropagation(); // ์์์์ ๋ถ๋ชจ๋ก ์ด๋ฒคํธ๊ฐ ํ๋ฌ๊ฐ๋ ๊ฒ์ ๋ฐฉ์ง์ํฌ ์ ์๋ค.
  		
  	} else {
  		event.cancelBubble = true;
  	}
  }
  ```

#### ๐ ์ด๋ฒคํธ ํธ๋ค๋ฌ ํจ์

โ	์ด๋ฒคํธ๊ฐ ๋ฐ์ํ์ ๋ ํธ์ถ๋๋ ํจ์๋ฅผ ์ด๋ฒคํธ ํธ๋ค๋ฌ ํจ์๋ผ๊ณ  ํ๋ค. 

#### ๐ JavaScript ์ด๋ฒคํธ ์ข๋ฅ

##### 1. onclick

โ	ํด๋ฆญ ์ด๋ฒคํธ๊ฐ ๋ฐ์ํ์ ๋ ์ ํด๋์ ์ฝ๋๊ฐ ์คํ๋๋๋ก ํ๋ค.

##### 2. onload / onunload

โ	์ฌ์ฉ์๊ฐ ์น ํ์ด์ง์ ์ง์ํ๊ฑฐ๋ ์น ํ์ด์ง๋ฅผ ๋ ๋  ๋ onload์ onunload ๋ฐ์.

##### 3. onchange 

โ	ํด๋น ์์์ ๋ณํ๊ฐ ๋ฐ์ํ์ ๋ ์ ํด๋์ ์ฝ๋๊ฐ ์คํ๋๋๋ก ํ๋ค.

##### 4. onmouseover

โ	์ฌ์ฉ์๊ฐ ๋ง์ฐ์ค๋ฅผ ์์ ์์ ์ฌ๋ฆฌ๋ฉด ์ด๋ฒคํธ๊ฐ ๋ฐ์ํ๋ค. 

#### ๐ this 

* `์ผ๋ฐํจ์์์ this`๋ `์๋์ฐ ๊ฐ์ฒด` ๋ค.
* `์๋์ฐ ๊ฐ์ฒด`๋ `์ ์ญ๋ณ์`๋ฅผ ์๋ฏธํ๋ค.
* `์์ฑ์ ํจ์๋ ํ๋กํ ํ์ ๋ฉ์๋`์์ this๋ ์๊ธฐ์์ ๊ฐ์ฒด๋ฅผ๊ฐ๋ฆฌํจ๋ค.
* `์ด๋ฒคํธ ํธ๋ค๋ฌ ํจ์` ๋ด์์ `this`๋ ์ด๋ฒคํธ๊ฐ ๋ฐ์ํ ๋์์ด๋ค.

#### ๐ ์ด๊ฒ์ ๊ฒ.. 

* `window.open('์คํํ  ๋ฌธ์์ด๋ฆ', '์ฃผ์์ฐฝ์ ๋์ธ ๋ฌธ๊ตฌ', 'ํฌ๊ธฐ') ` : ์์ ์๋์ฐ ์คํ
* `window.opener` : opener์ ์ด์ฉํ๋ฉด ๋ถ๋ชจ window์ ์ ๊ทผํ  ์ ์๋ค. 
* `innerText` : ์์ ์์ ํ์คํธ ๊ฐ๋ง ๊ฐ์ ธ์จ๋ค. 
* `innerHTML` : element ์์ html์ด๋ xml์ ๊ฐ์ ธ์จ๋ค.

