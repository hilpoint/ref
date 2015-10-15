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
      var x = new Boolean(false);
      var y = false;
      
      if (x) {
        console.log(x);
      }
      
      if (y) {
        console.log(y);
      }  
    ```

## 프로그래밍 프랙티스

### 5. UI 레이어 느슨하게 연결하기

+ css에서 자바스크립트 분리
+ 자바스크립트에서 css 분리
+ 자바스크립트에서 html 분리

### 6. 전역 선언 방지

+ 전역변수를 선언 하면 브라우저의 window  객체의 프로퍼티가 된다.
+ 특별한 경우가 아니면 전역 선언을 하지말자(디버깅, 테스트에 어려움)
+ One-Global 접근법(하나의 전역 객체만 사용)
  - jQuery , YUI 등에서 사용
    ```javascript
    /*
     * 일반적인 객체 생성 코드
     * 총 4개의 객체가 생성 됨
     */ 
    function Book(title) {
      this.title = title;
      this.page = 1;
    }
    
    Book.prototype.turnPage = function(direction) {
      this.page += direction;
    };
    
    var Chapter1 = new Book("1 class");
    var Chapter2 = new Book("2 class");
    var Chapter3 = new Book("3 class");
    
    /*
     * One Global
     * MainJS 객체 하나만 생성
     */ 
    var MainJS = {};
    
    MainJS.Book = function(title) {
      this.title = title;
      this.page = 1;
    };
    
    MainJS.Book.prototype.turnPage = function(direction) {
      this.page += direction
    };
    
    MainJS.Chapter1 = new MainJS.Book("1 class");
    MainJS.Chapter2 = new MainJS.Book("2 class");
    MainJS.Chapter3 = new MainJS.Book("3 class");
    ```

  + Zero-Global 접근법
    - 스크립트가 작고 페이지에 영향을 주지 않을 때 주로 사용
    - 북마클릿을 만들때 가장 많이 쓰인다.
  
### 7. 이벤트 처리

+ 애플리케이션 로직을 분리하자.
  ```javascript
  // 나쁜 예 - 이벤트 핸들러가 애플리케이션 로직을 포함한다.
  function handleClick(event) {
    var popup = document.getElementById("popup");
    popup.style.left = event.clientX + "px";
    popup.style.top = event.clientY + "px";
    popup.className = "reveal";
  }
  
  // 좋은 예 - 로직 분리
  var MyApp = {
    handleClick: function(event) {
      this.showPopup(event);
    },
    
    showPopup: function(event) {
      var popup = document.getElementById("popup");
      popup.style.left = event.clientX + "px";
      popup.style.top = event.clientY + "px";
      popup.className = "reveal";  
    }
  };
  
  addListener(element, "click", function(event) {
    MyApp.handleClick(event);
  }
  
  addListener(element, "click", handleClick);
  
  ```
+ 이벤트 객체를 바로 전달하지 말자.
  ```javascript
  // 좋은 예
  this.showPopup(event.clientX, event.clientY);
  ```

### 8. Null 비교 금지

+ 기본 데이터 타입 알아내기
  - 자바스크립트는 문자열, 숫자, 불린, null, undefiend 5개의 기본 데이터 타입이 있다.
  - typeof 연산자를 사용해 데이터 타입을 알수있다.(null은 object를 반환하므로 일치 연산자를 사용하자)
  - typeof variable 문법 사용을 권장.
  - typeof 연산자는 선언되지 않은 변수에 사용해도 에러가 발생하지 않는다.
+ 객체 참조 타입 알아내기
  - 기본 데이터 타입(null제외)이 아닌 모든 값은 참조값이며 typeof를 사용하면 object 문자열을 반환한다.(Object, Array, Date, Error 등)
  - 참조 타입은 instanceof 연산자를 사용하자
  - 값 instanceof 생성자명
    ```javascript
      if (value instanceof Error) {
        
        // code
        throw value;
      }
  ```
  - 모든 객체는 Object를 상속받으므로 어떠한 객체든 instanceof Object를 사용하면 true 값을 반환한다.
  - 함수는 참조 타입으로 Function 생성자가 존재하며 모든 함수는 Function의 인스턴스 이다.
  - 함수인지 확인 할때는 typeof 연산자를 사용하자
    ```javascript
      function myF() {}
      
      // ie8 이하에선 리턴값이 object 문자열로 반환되므로 주의하자
      console.log(typeof myF === "function");
    ```
  - 배열 알아내기 
    1. 더글라스 크락포드의 덕타이핑(sort() 메서드가 있는 객체는  배열밖에 없는것을 착안)
    
      ```javascript
        function isArray(value) {
          return typeof value.sort === "function";
        }
      ```
    2. 유리 자이체프 방법(주어진 값에서 네이티브 toString()을 하면 모든 브라우저에서 표준 문자열을 반환)
  
      ```javascript
        function isArray(value) {
          return Object.prototype.toString.call(value) === "[object Array]";
        }
      ```    
    3. ECMASript5에 공식 추가된 메서드(ie9, 파폭4, 사파리5, 오페라10.5 이상 지원)
    
      ```javascript
        function isArray(value) {
        
          // isArray라는 함수가 정의되어 있으면
          if (typeof Array.isArray === "function") {
            return Array.isArray(value);
          } else {
            return Object.prototype.toString.call(value) === "[object Array]";
          }
        }
      ```        
+ 프로퍼티 알아내기    
  - 프로퍼티가 존재하는지 확인 할때는 in 연산자를 사용하는 것이 가장 좋다.
    ```javascript
      var human = {
        age : 10,
        sex : null
      }
      
      // 좋은 예
      if ("age" in human) {
        // code
      }
      
      // 나쁜 예: false 값으로 검사
      if (object["age"]) {
        // code
      }    
      
      // 좋은 예
      if ("sex" in object) {
        // code
      }    
      
      // 나쁜 예 : null 비교
      if (object["sex"] != null) {
        // code
      }
    ```    
  - 상속받은 프로퍼티는 제외하고 객체 인스턴스에 프로퍼티가 있는지 검사하려면 hasOwnProperty() 메서드를 사용하자.
  - ie8 이하 버전의 DOM 객체는 Object를 상속하지 않아 메서드를 사용전에 있는지 확인해야 한다.
    ```javascript
      // DOM 객체
      if (object.hasOwnProperty("sex")) {
      
      }
      
      // DOM 객체인지 확실 하지 않을때
      if ("hasOwnProperty" in object && object.hasOwnProperty("sex")) {
      
      }      
    ``` 
  
### 9. 코드에서 구성 데이터 분리하기

+ 구성 데이터란 애플리케이션 코드에 직접 입력된 값이다.
  - URL
  - UI에 보여지는 문자열
  - 반복되는 고유한 값
  - 설정 값(ex: 페이지당 게시물 수)
  - 변경될 수 있는 값
+ 구성 데이터 분리하기
  ```javascript
    // 컨피그 객체에 모든 구성 데이터를 프로퍼티로 적용
    var config = {
      MSG_INVALID_VALUE : "Invalid value",
      URL_INVALID : "/error/invalid.php",
      CSS_SELECTED : "selected"
    }
  ```
+ 구성 데이터 저장하기
  - 자바스크립트에 구성 데이터를 두는 방법은 좋지 않다.
  - 자바프로퍼티 파일 형식으로 저장하는 것을 선호
  - 저장한 파일을 스크립트에서 사용할 수 있게 파일을 변환
    1. JSON 방식(다른 파일에 데이터를 삽입하거나 서버에서 데이터를 가져올 때 유용)
    2. JSONP 구조. JSON 구조를 함수로 감싸 반환하는 방식
    3. 변수에 JSON 객체를 할당하는 방식

### 10. 사용자 에러 던지기

+ 에러 던지기
  ```javascript
    // Error 객체를 사용해 에러를 던지자.
    throw new Error("에러 났다!");
    
    // ie8 이하 버전에서는 undefined 메시지를 표시..
    throw { name: "hil" }
    throw true;
    throw 1234;
    throw new Date();
  ```
+ 에러를 던지면 브라우저에서 정확한 메시지를 볼 수 있고 빠르고 정확한 디버깅이 가능하다.
+ 에러는 언제 던져야 할까?(에러를 막는 것이 아니라 에러가 발생하면 더욱 편하게 디버깅하는데 의의)
  - 디버깅하기 어려운 에러를 수정하면 거기에 사용자 정의 에러를 추가하자. 문제가 다시 발생하면 해결하는데 도움이 된다.
  - 코드를 작성할 때, 발생하면 안 된다고 생각하는 일이 발생하면 에러를 던진다.
  - 모르는 사람이 사용할 코드를 작성할 때는 함수를 잘못 사용할 수 있는 경우를 생각해보고 그 경우에 에러를 던지도록 하자.
+ try catch 문
  - 에러가 발생할 것으로 예상되는 코드에 try catch 문을 이용하면 브라우저가 에러를 처리하기 전에 에러를 먼저 가저 올수 있다.
+ 에러 타입(ECMA-262에는 총 7가지 타입의 에러를 정의함.)
  - ***Error***
    * 모든 에러의 기본 타입. 엔진에서 이 타입의 에러는 발생하지 않는다.
  - ***EvalError***
    * eval()에서 실행한 코드에 실행 중 에러가 있으면 이 타입으로 에러가 발생함.
  - ***RangeError***
    * 숫자가 범위를 벗어나면 이 타입 에러가 발생함. 예를 들어 -20개의 요소를 가진 배열을 생성하려고 new Array(-20)이라 하면 RangeError이 발생. 정상적으로 실행된다면 거의 발생하지 않는 에러.
  - ***ReferenceError***
    * 사용하려는 객체를 사용할  수 없을 때 발생한다. 예를 들어 null을 참조하는 변수에서 메서드를 호출하면 발생한다.
  - ***SyntaxError***
    * eval()에 전달한 코드가 문법상 문제가 있으면 발생함.
  - ***TypeError***
    * 변수가 알 수 없는 타입일 때 발생함. 예를 들어 new 10 또는 "prop" in true 같은 코드 실행시 발생함.
  - ***URIError***
    * 잘못된 형식의 URI 문자열이 encodeURI, encodeURIComponent, decodeURI, decodeURIComponent에 전달되면 발생함.
  - 사용자 정의 에러 및 정밀한 에러 검사를 하자
    ```javascript
      function MyError(message) {
        this.message = message;
      }
      
      MyError.prototype = new Error();
      
      try {
      
      } catch (ex) {
        if (ex instanceof TypeError) {
          // 기본 에러 처리
        } else if (ex instanceof ReferenceError) {
          // 기본 에러 처리
        } else if (ex instanceof MyError) {
          // 사용자저의 에러 처리
          // ie8 이하에서는 에러 메시지가 보이진 않지만 일반적인 에러 메시지인 "Exception thrown but not caught"가 뜬다.
          // 사용자 정의 에러 객체가 우리가 발생시킨 에러인지 구분할 수 있는 장점이 있다.
        } else {
        
        }
      }
    ```
