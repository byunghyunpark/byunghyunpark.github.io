---
layout: post
comments: true
categories: javascript
tags: Javascript library
---

> 1. 타이머 함수
> 2. Math 클래스
> 3. String 클래스
> 4. Data 클래스
> 5. Array 클래스 

<br>

# 타이머 함수

타이머 함수는 모두 전역 객체인 window에 들어 있습니다.

| 함수            | 설명                                           |
|-----------------|------------------------------------------------|
| setInterval()   | 일정 시간마다 주기적으로 특정 구문을 실행      |
| setTimeout()    | 일정 시간이 지난 후 특정 구문을 딱 한번만 실행 |
| clearInterval() | 실행 중인 타이머 함수를 멈춤                   |

<br>

## time 예제

```javascript
var $fish = null;
var step = 50;
var timeID = null;

function init(){
	$fish = $("#fish");
}

function initEvent(){
	$("#btnStart").click(function(){
		start();
	});

	$("#btnStop").click(function(){
		stop();
	});
}

function start(){
	timeID = setInterval(function(){
		moveFish();
	}, 100);
}

function stop(){
	clearInterval(timeID);
}

function moveFish(){
	// 다음 물고기 위치
	var x = $fish.position().left+step;

	// 물고기 위치가 430을 넘는 경우, 물고기 이동방향을 뒤쪽으로 변경 해줌
	if(x>=430){
	x=430;
	$fish.attr("src","fish2.png");
		step=-50;
	}

	// 물고기 위치가 50보다 작은 경우, 물고기 이동방향을 앞쪽으로 변경 해줌
	if(x<50){
		x=50;
		$fish.attr("src","fish1.png");
		step=50;
	}
	 // 물고기 위치 설정
	$fish.css("left", x);
}

$(document).ready(function(){
	init();
	initEvent();
});
```
<br>
<hr>
# Math 클래스


## 자주 사용하는 메서드 목록

| 메서드   | 설명                         |
|----------|------------------------------|
| abs()    | 숫자의 절댓값 반환           |
| ceil()   | 숫자의 올림값 반환           |
| max()    | 주어진 두 수 중 큰 값 반환   |
| min()    | 주어진 두 수 중 작은 값 반환 |
| random() | 0과 1 사이의 난수 값을 반환  |
| round()  | 숫자의 정수 반올림           |

<br>
## random 예제

```javascript
// 0.5초 마다 50 ~ 100 사이 정수를 출력하기

var $info = null;
$(document).ready(function() {
    $info = $("#info");

    showRandom();
    setInterval(showRandom, 500);
})

function showRandom() {
    var value = parseInt(Math.random() * 50) + 50;
    $info.html(value);
}
```

```javascript
// 랜덤으로 이미지 변경하기

$(document).ready(function(){
    var imgList1 = ["logo_01.jpg","logo_02.jpg","logo_03.jpg","logo_04.jpg","logo_05.jpg"];
    var imgList2 = ["logo_02.jpg","logo_05.jpg","logo_03.jpg","logo_04.jpg","logo_01.jpg"];

    // 첫 번째 배너
    banner("#banner1", imgList1, 1000);
    banner("#banner2", imgList2, 3000);
})

// 배너
function banner(selector, imgList, speed){

    var $banner = $(selector);

    // 1초에 한번씩 함수 호출
    setInterval(function(){
		 var currentIndex = Math.floor(Math.random()*imgList.length)

        // 다음 이미지 이름 구하기
        var imgName = imgList[currentIndex];
        
        // 다음 이미지 출력
        $banner.attr("src", "./images/"+imgName);
    },speed)
}
```
<br>


## max/min 예제

```javascript
// 10 ~ 100 사이 숫자 입력 validation

var value = window.promp("숫자입력", 0);
value = Math.min(100,Math.max(10,value));
alert(value);

```
<br>


## time & random 복합예제

```javascript
var count=0;
var $score=null;
var $fish=null;
var $start=null;
var playing=false;
var timeID=null;

// 도큐먼트 로드
$(document).ready(function(){
    init();
    initEvent();
});

// 요소 초기화
function init(){
  $score = $("#score");
  $fish = $("#fish");
  $start = $("#start");
}

// 이벤트 초기화
function initEvent(){
  $start.click(function(){
    startGame();
  })

  // 물고기에 클릭 이벤트 등록
  $fish.click(function(){
    countUp();
  });
}

// 게임시작
function startGame(){
  if(playing==false){
    endGame();
    playing=true;
    timeID = setInterval(function(){
      moveFish();
    },1000);
  }
}

// 게임종료
function endGame(){
  // 게임을 5초후에 종료시켜 줍니다.
  setTimeout(function(){
      playing=false;
      clearInterval(timeID);
      alert("게임이 종료 되었습니다.")
  },5000);
}

// 물고기 이동
function moveFish(){
  var x = Math.floor(Math.random()*480);
  var y = Math.floor(Math.random()*330);

  $fish.css({
    left:x,
    top:y
  })
}

// 점수 올리기
function countUp(){
  if(playing==true){
      // 점수 증가
      count++;
      $score.html(count);
  }
}
```

<br>
<hr>

# String 클래스

## 주요 메서드

| 메서드                | 설명                                                       |
|-----------------------|------------------------------------------------------------|
| charAt(n)             | n번째 문자 구하기                                          |
| charCodeAt(n)         | n번째 코드 값 구하기                                       |
| concat(str)           | 문자열 뒤쪽에 str을 연결해 새로운 문자열 만들기            |
| indexOf(substr)       | substr 문자열이 위치한 위치값 구하기, 앞에서부터 검색 시작 |
| lastIndexOf(substr)   | substr 문자열이 위치한 위치값 구하ㄱ, 뒤에서부터 검색 시작 |
| match(reg)            | 정규표현식을 활용한 문자열 검색                            |
| replace(reg,rep)      | 정규표현식을 활용한 문자열 교체                            |
| search(reg)           | 정규표현식을 활용한 문자열 위치 검색                       |
| slice(start,end)      | start번째부터 end번째 문자열 추출                          |
| split(str)            | 문자열을 str로 분할해 배열로 생성해줌                      |
| substr(start[,count]) | start번째부터 count 개수만큼 문자열 추출                   |
| toLowerCase()         | 모든 문자열을 소문자로 변환                                |
| toUpperCase()         | 모든 문자열을 대문자로 변환                                |
| trim()                | 좌우 공백 제거                                             |

<br>

## string 공백제거 예제

```javascript
$(document).ready(function(){
    var $output = $("#output");
    var $input = $("#input");

    $("#confirm").click(function(){
       // 입력 값 알아내기
       var value = $input.val();

       var result = ltrim(value);
       $output.html(value+" ==> "+result);
    })
});


// 앞쪽 공백문자를 제거하는 함수
function ltrim(str){

    // 공백 pass
    if(str.length<=0){
      return "";
    }

    // 마지막에 공백이 없는경우 pass
    var lastCh = str.charAt(str.length-1);
    if(lastCh!="_"){
      return str;
    }

    // 문자 index 찾기
    for(var index=str.length-1; index>=0; index--){
      var ch = str.charAt(index);
      if(ch!="_"){
        index++;
        break;
      }
    }

    // 공백제거 후 리턴
    var result = str.substr(0, index);
    return result;
}
```

<br>

## 세자리 자릿수 "," 예제

```javascript
$(document).ready(function(){
    var $output = $("#output");
    var $input = $("#input");

    $("#confirm").click(function(){
        // 입력 값 알아내기
        var value = $input.val();

        //3자릿수 마다 콤마(,) 추가
        var result = won(value);

        // 결과 출력
        $output.html(value+" ==> "+result);
    })
});

//3자릿수 마다 콤마(,) 추가
function won(value){

    // 3자 이하 pass
    if(value.length<=3){
      return value;
    }

    var result = "";

    // 루프 횟수 구하기
    var roofCount = Math.floor((value.length-1)/3);

    for(var i=0; i<roofCount; i++){

      // 길이 초기화
      var length = value.length;
      // 뒤에서 3개 자르기
      var strCut = value.substr(length-3, length);
      // 합치기
      result = ","+strCut+result;
      // value 업데이트
      value = value.slice(0, length-3);
    }
    // 앞자리 나머지 붙여서 리턴
    result = value + result;
    return result;
}
```
<br>
<hr>

# Date 클래스

## 주요 메서드

| 메서드        | 설명                                            |
|---------------|-------------------------------------------------|
| getDate()     | 로컬시간을 사용하여 일(월 기준)을 반환          |
| getDay()      | 로컬시간을 사용하여 일(주 기준, 즉 요일)을 반환 |
| getMonth()    | 로컬시간을 사용하여 월을 반환                   |
| getFullYear() | 로컬시간을 사용하여 연도을 반환                 |
| getSeconds()  | 로컬시간을 사용하여 초를 반환                   |
| getMinutes()  | 로컬시간을 사용하여 분을 반환                   |
| getHours()    | 로컬시간을 사용하여 시간을 반환                 |
| setDate()     | 로컬시간을 사용하여 일(월 기준)을 설정 |
| setMonth()    | 로컬시간을 사용하여 월을 설정          |
| setFullYear() | 로컬시간을 사용하여 연도를 설정        |
| setSeconds()  | 로컬시간을 사용하여 초를 설정          |
| setMinutes()  | 로컬시간을 사용하여 분을 설정          |
| setHours()    | 로컬시간을 사용하여 시간을 설정        |

<br>

## Date 메서드 관련 시계예제
```javascript
$(document).ready(function() {
    init();
    clock();
});

function init(){
  $output = $("#output");
}

function clock(callback){
  getTime(print1);
  setInterval(function(){
    getTime(print1);
  }, 500);
}

function addZero(value){
  if(value<10){
    value = "0"+value;
  }
  return value;
}

function getTime(callback){
  var objDate = new Date();

  var hours = addZero(objDate.getHours());
  var minutes = addZero(objDate.getMinutes());
  var seconds = addZero(objDate.getSeconds());

  var year = objDate.getFullYear();
  var month = objDate.getMonth();
  var date = objDate.getDate();

  var day = objDate.getDay();
  var dayArray = ["일","월","화","수","목","금","토"];

  callback(year, month, date, dayArray, day, hours, minutes, seconds);
}

function print1(year, month, date, dayArray, day, hours, minutes, seconds){
  $output.html(
    year+"년 "+month+"월 "+date+"일 "+dayArray[day]+"요일<br>"+
    hours+":"+minutes+":"+seconds
  );
}

function print2(year, month, date, dayArray, day, hours, minutes, seconds){
  $output.html(
    hours+":"+minutes+":"+seconds
  );
}
```

<br>
<hr>

# Array 클래스

## 주요 메서드

| 메서드                     | 설명                                                         |
|----------------------------|--------------------------------------------------------------|
| concat()                   | 배열에 다른 배열이나 값을 연결해 새 배열을 반환              |
| indexOf()                  | 배열 요소의 인덱스 값을 반환, 배열 요소가 없는 경우 -1 반환  |
| join([separator])          | 지정된 구분 문자열로 배열 요소들을 이어 붙여서 문자열로 만듦 |
| pop()                      | 마지막 배열 요소 반환                                        |
| push(element[,element])    | 새로운 배열요소를 마지막 배열 위치에 추가                    |
| reverse()                  | 배열 요소의 순서를 반대로 바꿈                               |
| shift()                    | 배열에서 첫 번째 요소를 제거한 후 배열을 반환                |
| slice()                    | 배열의 일부분을 반환                                         |
| sort([compareFunction])    | 배열 요소를 내림차순 또는 오름차순으로 정렬                  |
| splice(start, deleteCount [,element])                   | 배열 요소를 추가, 삭제, 교체                                 |
| unshift(element[,element]) | 배열 맨 앞에 배열 요소를 삽입                                |
| split([separator])         | 문장을 [separator]기준으로 나눠서 배열로 반환                |

<br>

## splice 용법
> 삭제가 없을 경우 deleteCount는 0으로 설정합니다.

```javascript
var menuItems = ["menu1", "menu2", "menu3", "menu4"];
console.log("실생전 : " + munuItems.join(","));
menuItems.splice(2,0,"new");
console.log("실행후 : " + munuItems.join(","));

// 결과
// 실행 전 : menu1, menu2, menu3, menu4
//	실행 후 : menu1, menu2, new, menu3, menu4

```
<br>

## sort 용법
> 숫자 오름차순의 경우 sort()로 정렬 할 수 없습니다.

```javascript
// 문자 오름차순
arrayData.sort()
// 문자 내림차순
arrayData.sort(function(a,b){ return b>a; });
// 숫자 오름차순
arrayData.sort(function(a,b){ return a-b; });
// 숫자 내림차순
arrayData.sort(function(a,b){ return b>a; });
```

<br>
<hr>

reference
- 자바스크립트+jQuery 완전적복 스터디(김춘경)


