---
layout: post
comments: true
categories: javascript
tags: Javascript
---
> 자바스크립트 함수 기초내용입니다.


# 중첩 함수

함수 안에 함수가 속해있는 구조입니다.

중첩 함수에서는 중첩함수를 포함하고 있는 함수의 지역변수에 접근해서 사용할 수 있습니다.

```html
<script>
    var a=10;
    var b=20;
    var c=30;
    function outer_func(){
        var b=200;
        var c=300;
        function inner_func(){
            var c=3000;
            document.write("1. a = "+a+"<br>");
            document.write("2. b = "+b+"<br>");
            document.write("3. c = "+c+"<br>");
        }
        inner_func();
    }
    outer_func();

    /*
     결과 :
         1. a = 10
         2. b = 200
         3. c = 3000

     */
</script>
```

<br>

# callback 함수
로직의 결과값만 다르게 출력할때 유용합니다.

```html
<!DOCTYPE HTML>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Untitled Document</title>

    <script>

        // 사칙연산 함수
        function calculator(op, num1,num2, callback){
            var result="";

            switch(op) {
                case "+" :
                    result = num1 + num2;
                    break;
                case "-" :
                    result = num1 - num2;
                    break;
                case "*" :
                    result = num1 * num2;
                    break;
                case "/" :
                    result = num1 / num2;
                    break;

                default :
                    result = "지원하지 않는 연산자입니다";
            }

            callback(result);
        }

        function print1(result) {
          document.write("두 수의 합은"+result+"입니다.", "<br>");
        }

        function print2(result) {
          document.write("정답은"+result+"입니다.", "<br>");
        }

        calculator("+", 10,20, print1);
        calculator("+", 10,20, print2);


    </script>



</head>

<body>

</body>
</html>
```

<br>

**콜백함수는 보통 비동기 함수의 결과 값을 처리하기 위해 많이 사용됩니다.**

> 비동기란?
> 
> 함수가 호출된 후 끝날 때까지 기다리지 않고 바로 다음 구문을 실행하는 경우를 비동기라고합니다.
> 
> ```html
> var count=1;
> 
> setInterval(function(){
>     document.write("2. count = "+count);
> },3000);
> 
> document.write("1. ajax() 동작이 모두 끝나지 않았어도 바로 실행됩니다.");
> ```
> 
> setInterval() 함수가 실행되면 자바스크립트 엔진은 동기 함수와는 달리 3초를 기다리지 않고 바로 다음 구문을 실행합니다.

<br>

**콜백함수는 다음과 같은 경우에 많이 사용됩니다.**

1. 이벤트 리스터로 사용

	```javascript
	$("#btnStart").click(function(){
	      alert("클릭되었습니다.");
	    });
	```

2. 타이머 실행 함수로 사용
	
	```javascript
	setInterval(function(){
      alert("1초마다 한 번씩 실행되요");
    });
	```
3. Ajax 결과값을 받을 때 사용

	```javascript
	$.get("http://luxlab.co.kr/", function(){
	      alter("정상적으로 서버 통신이 이뤄졌습니다.");
	    });
	```

4. jQuery 애니메이션 완료

	```javascript
	$("#target").animate ({
	      left:100,
	      opacity:1
	    },2000,"easeoutQuint", function(){
	      alert("애니메이션이 완료되었습니다.");
	    });
	```

<br>
	
# 클로저 함수

클로저는 쉽게 말하면 함수 내부에서 만든 지역변수가 사라지지 않고 계속해서 값을 유지하고 있는 상태를 말합니다. 내부함수에서 내부함수를 포함하고 있는 외부함수의 변수A를 사용하는 구조인 경우로 표현할 수 있으며 내부함수를 클로저 함수라고 부릅니다. 또한 변수A는 클로저 현상에 의해 외부함수 호출이 끝나더라도 사라지지 않고 값을 유지합니다.

```javascript
// 일반 함수인 경우
function addCount(){
    var count=0;
    count++;
    return count;
}

document.write("1. count = "+addCount(),"<br>");
document.write("2. count = "+addCount(),"<br>");
document.write("3. count = "+addCount(),"<br>");

/*
 실행결과 :
 1.	Count = 1
 2.	count = 1
 3.	count = 1
 */
```

```javascript
// 클로저를 사용한 경우
function createCounter(){
    var count=0;
    function addCount(){
        count++;
        return count;
    }
    return addCount;
}

var counter = createCounter();

document.write("1. count = " + counter(),"<br>");
document.write("2. count = " + counter(),"<br>");
document.write("3. count = " + counter(),"<br>");


/*
 실행결과 :
 1.	count = 1
 2.	count = 2
 3.	count = 3
 */
```

```javascript
// 리턴이 없는 클로저함수

$(document).ready(function() {
    $("#btnStart").click(function () {
        start();
        document.write("시작합니다.");
    });

    function start() {
        var count = 0;
        setInterval(function () {
            count++;
            document.write(count);
        }, 1000);
    }
});
```
**변수가 사라지지 않고 계속해서 값을 유지하는 상태를 모두 클로저라고 부릅니다.**
아래 예제는 $selectMenuItem 변수가 클로저 변수로 같은 함수를 호출한 두 개의 탭이 각각 다른 변수를 저장하게 됩니다.

```html
<!DOCTYPE HTML>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title></title>

	<style>
		.tab-menu {
			list-style: none;
			height:80px;
		}

		.tab-menu li {
			width:99px;
			height:40px;
			background-position-y:0;
			text-indent: -1000px;
			overflow: hidden;
			display: inline-block;
			float:left;
		}
		.tab-menu li:hover {
			background-position-y: -40px;
		}
		.tab-menu li.select {
			background-position-y: -80px;
			height:80px;
		}
		.tab-menu li.menuitem1 {
			background-image: url(./images/newbtn.bar.1.png);
		}
		.tab-menu li.menuitem2 {
			background-image: url(./images/newbtn.bar.2.png);
		}
		.tab-menu li.menuitem3 {
			background-image: url(./images/newbtn.bar.3.png);
		}
		.tab-menu li.menuitem4 {
			background-image: url(./images/newbtn.bar.4.png);
		}
		.tab-menu li.menuitem5 {
			background-image: url(./images/newbtn.bar.5.png);
		}
		.tab-menu li.menuitem6 {
			background-image: url(./images/newbtn.bar.6.png);
		}
	</style>
	 
    <script src="../../libs/jquery-1.11.0.min.js"></script>
    

	<script>
		$(document).ready(function(){
			// 탭메뉴 코드가 동작할 수 있도록 tabMenu() 함수 호출
			tabMenu("#tabMenu1 li");

			tabMenu("#tabMenu2 li");
		});


		function tabMenu(selector){
			// 선택 한 탭메뉴를 저장할 변수
			var $selectMenuItem =null;

			// 메뉴 항목에 클릭 이벤트 등록
			$(selector).click(function(){

				// 기존 선택 메뉴 항목이 있으면 비 선택 상태로 만들기
				if($selectMenuItem!=null){
				   $selectMenuItem.removeClass("select");
				}

				// 클릭한 메뉴 항목을 신규 선택 메뉴 항목으로 저장
				$selectMenuItem = $(this);
				// 선택 상태로 만들기
				$selectMenuItem.addClass("select");

			})
		}
	</script>


</head>
	
	<body>
		<p>첫 번째 탭메뉴</p>	    
		<ul class="tab-menu" id="tabMenu1">
			<li class="menuitem1">google</li>
			<li class="menuitem2">facebook</li>
			<li class="menuitem3">pinterest</li>
			<li class="menuitem4">twitter</li>
			<li class="menuitem5">airbnb</li>
			<li class="menuitem6">path</li>
		</ul>		
		<p>두 번째 탭메뉴</p>
		<ul class="tab-menu" id="tabMenu2">
			<li class="menuitem1">google</li>
			<li class="menuitem2">facebook</li>
			<li class="menuitem3">pinterest</li>
			<li class="menuitem4">twitter</li>
			<li class="menuitem5">airbnb</li>
			<li class="menuitem6">path</li>
		</ul>	
	</body>
</html>
```


