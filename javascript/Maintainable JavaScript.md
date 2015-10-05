# 읽기 좋은 자바스크립트 코딩 기법 정리중
2015.10.01 잠이 들지 않는 이밤 공부중

## 1장. 기본 포멧

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


## 2장. 주석

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

### 3장. 문장과 표현식

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

