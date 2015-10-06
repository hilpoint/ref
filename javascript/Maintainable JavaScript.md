# 읽기 좋은 자바스크립트 코딩 기법 정리중
2015.10.01 잠이 들지 않는 이밤 공부중

## 스타일 가이드 라인

### 1장. 기본 포멧

+ 들여쓰기는 공백4개를 사용하기를 추천
+ 세미콜론을 명시적으로 넣기를 권장(**Auto Semicolon Insertion** 매커니즘이 있지만 사용하자!!)
+ 줄 길이는 한 줄당 80자 제한을 선호
+ 줄이 최대 글자수를 도달할시 연산자 다음에 줄을 바꾸어 두단계 들여쓰기를 하자
```javascript
  if (isTrue && isEmpty && isVisible && day == 30 && isBeautiful &&
          isKorean) {
          
          doSomething();
  }
  ```
+ 적재적소에 빈줄을 추가하자
  - 메서드 사이
  - 메서드 내 지역 변수와 첫 번째 문장 사이
  - 한줄 또는 여러줄 주석 전
  - 가독성을 높이기 위해 메서드 내에서 논리적으로 구분되는 곳
+ 이름규칙
  - 낙타표기법을 사용하되 변수명은 명사로 함수명, 메서드명은 동사로 시작하도록 하자

동사 | 의미
---- | ----
can | 불린값 반환 함수
has | 불린값 반환 함수
is | 불린값 반환 함수
get | 불린 이외의 값을 반환 함수
set | 값을 저장하기 위해 사용하는 함수

  - 상수는 모든 문자를 대문자로 쓰고 단어 바뀔때는 밑줄을 사용하자
  - 생성자는 명사 대문자로 시작한다.
  - null은 한정된 곳에서만 사용하자
    + 나중에 값을 할당할 변수를 초기화할 때
    + 선언한 변수에 값이 할당되었는지 비교할 때
    + 인자 값으로 객체를 넘기는 함수를 호출할 때
    + 함수를 호출한 곳에서 반환값으로 객체를 기대할 때
    + 함수의 인자 값을 확인하기 위해 null과 비교하거나 초기화 되지 않을 변수와 null은 사용하지 말자


### 2장. 주석

+ 한줄 주석
  - 주석을 입력 하기전에 한줄 비우고 시작. 설명할 코드 바로 윗줄에 작성. 코드에 들여쓰기 맞춤
  - 줄 끝에 꼬리 주석은 한단계 들여쓰기 한후 입력. 최대 줄 길이 넘지 안도록.
  - 코드 주석 처리시 사용

```javascript
// 좋은 예
if (isNotNull) {

    // 이 문장에 오면 Null이 아님
    doSomething();
}
```
+ 여러 줄 주석 
  - 한줄 비우고 시작. 설명할 코드 바로 윗줄에 작성. 들여쓰기 맞춤
+ 이해하기 어렵거나 오해하기 쉬운 코드 그리고 특정 브라우저 핵에는 주석설명을 잘 달자.
+ 문서화 주석
  - 문서화 주석을 사용할 때에는 모든 메서드, 모든 생성자, 주석을 추가한 메서드를 가진 모든 객체에 대한 설명이 있어야한다.
```javascript
/**
JAVADOC 문서 포멧으로 많이 쓴다.

@param {Object} isNotNull 널인지 확인 true/false
@return {Object} 문자열 값 
**/
if (isNotNull) {

    // 이 문장에 오면 Null이 아님
    return "isNotNull";
}
```

#### 3장. 문장과 표현식

+ 다소 취향이 갈리지만 선호하는 스타일을 정리(코딩스타일 관점은 서로다르므로)
+ 복합문에서 중괄호는 반드시 사용하자. 오해의 소지를 미연에 방지
+ 복합문 사이에 공백을 넣자
```javascript
  if (condition) {
    console.log("내가 선호하는 패턴");
  }
```
+ 스위치문은 break, default 등 미사용시 명확한 주석을 달자
```javascript
  // break 관련
  switch (condition) {
  
    // 아무로직없이 다음 case문으로 ..
    case "first":
    case "second":
      // code
      break;
    case "third"
      // code
      
      // jsLint에서 break 경고 메세지를 출력안함
      /*falls throuth*/ 
    default:
      // code
  }
  
  // default 관련
  switch (condition) {
    case "first":
      // code
      break;
    case "second":
      // code
      break;
    // no default
  }
```
+ with 문은 사용하지 말자
+ for in 반복문 사용시 프로토타입 체인을 사용하지 않는다면 hasOwnProperty() 메서드를 반드시 사용하자

#### 4장. 변수, 함수, 연산자

+ 자바스크립트에서 변수는 어느 곳에나 올수 있고 여러번 사용할 수 있다.
+ 모든 변수 선언은 선언위치와 상관없이 함수 최상단으로 끌어올려짐(**호이스팅**)
```javascript
  // 개발자가 작성 한 코드
  function doSomethingWithItems(items) {
    for (var i = 0, len = items.length; i < len; i++) {
      doSomething(items[i]);
    }
  }
  
  // ECMAScript5 엔진에서 해석한 코드
  function doSomethingWithItems(items) {
    var i,
        len;
    for (i = 0, len = items.length; i < len; i++) {
      doSomething(items[i]);
    }
  }  
```

+ 함수 선언도 변수와 마찬가지로 엔진에 의해 끌어 올려진다.
+ 함수를 사용하기 전에 선언할 것을 권장.
+ 함수는 블록안에서 선언하지 말자.(브라우저에 따라 동작할수도 안할수도 있음)
+ 함수 호출문에는 함수명과 괄호 사이에 공백을 입력하지 않는 스타일을 권장
  + ex) getName(user);
+ 익명 함수 선언문 끝에 괄호를 열고 닫으면 바로 호출할 수 있다.
```javascript
  // 나쁜 예
  var value = function () {
    
    // code
    
    // 객체를 리턴
    return {
      message: "Hi"
    }
  }();
  // 좋은 예
  var value = (function () {
    
    // code
    
    // 객체를 리턴
    return {
      message: "Hi"
    }
  }());  
```

+ 동등연산자
  - 숫자와 문자열 비교 시 문자열이 숫자로 변환되고 그 후 비교 연산 수행
  - boolean 값과 숫자 비교 시 boolean 값이 먼저 숫자로 변경되고 비교 연산 수행(false는 0, true는 1)
  - 객체를 객체가 아닌 값과 비교
    1. 객체의 valueOf() 메서드를 호출해서 얻은 기본 데이터 타입값으로 비교 연산 수행.
    2. valueOf()가 없으면 toString()을 호출.
    3. 그 이후부터는 비교 대상이 같은 타입이 아니면 타입 강제 변환을 수행후 비교 연산
  - 동등연산자는 !== 와 === 사용을 권장.
+ eval() 사용 금지.
+ 기본 래퍼 타입
  - String, Boolean, Number
  - 기본 래퍼 타입 직접 사용하지 말자.
```javacript

  /*
   *  둘 다 false를 의미 하지만 변수 x는 객체이므로 첫 번째 조건문에서는 undefined나 null이 아니므로 true로 판단.
   *  하지만 변수 y는 값이 false이므로 조건문을 통과 못함.
   *  따라서 기본 래퍼 타입 객체를 명시적으로 사용하면 찾기 어려운 버그가 탄생할 수 있음.
   */
  var x = new Boolean(true);
  var y = false;
  
  if (x) {
    console.long(x);
  }
  
  if (y) {
    console.long(y);
  }  
```
