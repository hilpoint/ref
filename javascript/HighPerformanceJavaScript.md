# 자바스크립트 성능 최적화 정리
2015.10.19 정리시작

### 1. 로딩과 실행

+ 브라우저는 script 태그를 만나면 처리를 중단하고 자바스크립트 코드들 실행한 다음 페이지 분석과 렌더링을 계속한다.
+ 여러개의 스크립트를 받으면 처음 스크립트 파일을 다운받고 해당코드를 실행한뒤 그다음 스크립트 파일을 받는 식으로 진행이 된다.
+ 스크립트의 위치
  - 가능한 body 태그안 마지막 라인에 스크립트를 설정하자. (html 페이지 렌더링후 다운로드)
+ 스크립트 묶기
  - 여러 스크립트 파일을 script 태그 하나로 불러오면 지연시간을 최소화하여 성능에 미치는 영향을 최소화 할수 있다.
+ 비차단 스크립트
  - 스크립트 태그에 defer 속성을 쓰자(인터넷 익스플로러와 파폭 3.5이상)
  - 스크립트 태그를 동적으로 생성해서 코드를 내려받아 실행하게 하자
  - XHR 객체로 자바스크립트 코드를 내려받아 페이지에 삽입하자(자바스크립트 파일과 그파일을 요청하는 페이지가 반드시 같은 도메인에 있어야하므로 CDN을 통해서 파일을 받을수 없는 단점)
  - LazyLoad , LABjs 라이브러리 
  
### 2. 데이터 접근

+ 자바스크립트에서 데이터를 저장할 수 있는 장소 4가지
  - 리터럴 값(문자열, 숫자, 불리언, 객체, 배열, 함수, 정규표현식, null, undefined)
  - 변수
  - 배열 항목
  - 객체 멤버
+ 스코프 관리
 - 함수는 Function의 인스턴스 이다.
 - 다른 객체와 마찬가지로 속성이 있는데 접근 가능한 속성과 불가능한 내부속성(***스코프***)으로 나뉜다.
  ```javascript
    /*
     * add() 함수가 생성될때 함수의 스코프 체인에 전역으로 정의된 
     * 변수 전체를 나타내는 전역 객체가 하나 들어간다.
     * 이 전역 객체는 window, navigator, document 등에 대한 참조가 들어가 있다.
     */
    function add(num1, num2) {
      var sum = num1 + num2;
      return sum;
    }
    
    /*
     * add() 함수를 실행 하면 실행문맥(execution context)이라는 내부 객체가 생성
     * 실행 문맥음 함수가 실행되는 환경을 정의 한다.
     * 함수를 실행 할때 마다 별도의 실행 문맥이 만들어 진다.
     * 함수가 완전히 실행 되면 실행 문맥은 파괴
     * 실행문맥도 식별자 해석(identifier resolution)에 쓸 자기 만의 스코프 체인을 생성하는데 
     * 실행 중인 함수의 스코프 속성에 있는 객체로 초기화 된다.(함수에 나타나는 순서대로 복사됨)
     * 이 과정이 끝나면 활성화 객체(Activation Object)라고 부르는 새로운 객체가 실행 문맥에 생성 된다.
     * 활성화 객체는 스코프 체인의 앞에 자리 잡으며 이 생핼에 대해 변수 객체 구실을 한다.
     * 모든 지역 변수, 명명된 매개변수, agguments 집합, this 항목을 포함.
     * 실행 문맥이 파괴 되면 활성화 객체도 파괴됨.
     */
    var total = add(5, 10);
    ```
  - 함수를 실행하는 도중 변수가 등장하면 이 변수와 같은 이름의 식별자를 찾기 위해 실행 문맥의 스코프 체인을 검색하는데 
    이 검색 과정이 성능을 떨어트린다.
  ![Image](http://figures.oreilly.com/tagoreillycom20090601oreillybooks300541I_book_d1e1/figs/I_mediaobject7_d1e6895-web.png)
  - 식별자 해석 성능은 함수 안에 있는 지역 변수에 가장 빠르게 접근하고 전역 변수에 접근 하는 것이 일반적으로 가장 느리다.
  - 따라서 함수 바깥쪽의 값을 한번 이상 사용한다면 항상 지역 변수에 저장하여 사용 하는 것이 좋다.
    ```javascript
      // 잘못된 예
      function initUI() {
        var bd = document.body,
            links = document.getElecmentsByTagName("a"),
            i = 0,
            len = links.length;
            
            // code ...
      }
      
      // 좋은 예 - 전역 변수에 한번만 접근
      function initUI() {
        var doc = document,
            bd = doc.body,
            links = doc.getElecmentsByTagName("a"),
            i = 0,
            len = links.length;
            
            // code ...
      }
    ```
  - 동적 스코프(with, try-catch 문의 catch 절, eval()이 포함된 함수)는 꼭 필요할 때만 쓰기를 권장.
  - 클로저는 메모리와 실행 속도 모두에 영향을 끼치므로 주의하여 사용하자.
+ 객체 멤버
  - 객체 멤버가 깊이 중첩 될수록 데이터 접근이 더 느려진다. (ex: window.location.href 보다 location.href가 더 빨리 해석)
  - 함수에서 객체의 속성을 두번 이상 읽을 거라면 그 값을 지역 변수에 저장하자
    * 객체 메서드들은 다수가 this를 통해 자신을 호출한 문맥을 판단하여 메서드를 지역 변수에 저장하면 this가 window에 묶이므로 객체 메서드는 이 방법을 쓰지 않는게 좋다.

### 3. DOM 스크립팅

+ DOM 접근과 수정
  - DOM 요소에 접근하기만 해도 비용이 발생한다.
  - 요소를 수정하면 브라우저가 페이지의 기하학적 구조를 다시 계산해야 할 경우가 잡아 더 큰 비용이 발생.
    ```javascript
      // 비효율적
      function innerHTMLLoop() {
        for (var count = 0; count < 15000; count++) {
          document.getElementById('here').innerHTML += 'a'
        }
      }
      
      // 효율적 - 페이지 내용을 마지막에서 한 번만 수정
      function innerHTMLLoop2() {
        var content = '';
        for (var count = 0; count < 15000; count++) {
          content += 'a';
        }
        document.getElementById('here').innerHTML += content;
      }        
    ```
  - 비표준이지만 innerHTML 이 document.createElement() 같은 DOM 메서드 보다 빠르다.
  - 노드 복제시 document.createElement() 보다 elemnet.cloneNode() 가 조금 더 빠르다.
  - 루프 제어 조건에서 배열의 length 속성에 접근 하는 것은 좋지 않다. 따라서 지역변수를 활용하자.
  - 컬렉션의 length 속성에 접근하는 것은 배열의 length 속성에 접근 하는 것보다 느리다.
    ```javascript
      // 느린 방법
      function collectionGlobal() {
        var coll = document.getElementsByTagName('div'),
            len = coll.length,
            name = '';
            
        for (var count = 0; count < len; count++) {
          name = document.getElementsByTagName('div')[count].nodeName;
          name = document.getElementsByTagName('div')[count].nodeType;
          name = document.getElementsByTagName('div')[count].tagName;
        }
        return name;
      };
      
      // 빠른 방법
      function collectionLocal() {
        var coll = document.getElementsByTagName('div'),
            len = coll.length,
            name = '';
            
        for (var count = 0; count < len; count++) {
          name = coll[count].nodeName;
          name = coll[count].nodeType;
          name = coll[count].tagName;
        }
        return name;
      };   
      
      // 가장 빠른 방법
      function collectionNodesLocal() {
        var coll = document.getElementsByTagName('div'),
            len = coll.length,
            name = '',
            el = null;
            
        for (var count = 0; count < len; count++) {
          el = coll[count];
          name = el.nodeName;
          name = el.nodeType;
          name = el.tagName;
        }
        return name;
      };  
    ```
+ 리페인트와 리플로우
  - 브라우저가 페이지의 구성 요소 전체(HTML 마크업, 자바스크립트, css, 이미지)를 내려받으면 파일을 분석해 내부적인 데이터 구조 두가지를 만든다.
    * DOM 트리 : 페이지 구조를 나타낸 것
    * 렌더 트리 : DOM 노드를 어떻게 표시할 지 나타낸 것
  - **리플로우** : 브라우저가 렌더 트리 중에서 변경(너비와 높이 같은 기하학적 구조)에 영향을 받은 부분을 유효하지 않은 것으로 간주하고 렌더 트리를 다시 만드는 과정.
    * 보이는 DOM 요소를 추가했거나 제거했을 때
    * 요소의 위치가 바뀌었을때
    * 요소의 크기가 바뀌었을 때(마진, 패딩, 테두리 두께, 너비, 높이 등이 바뀌었을 때)
    * 텍스트 내용이 바뀌거나 이미지가 다른 크기의 이미지로 대체되는 등 내용이 바뀌었을 때
  - **리페인트** : 리플로우가 끝나고 브라우저가 영향을 받는 부분을 다시 그리는 과정.
  - 스타일 변경은 한 번에 묶어서 처리하고, DOM 트리를 조작할 때는 레이아웃 흐름에서 분리하여 작업하고 레이아웃 정보는 캐시하고 최소한으로 쓰는 등 리페인트와 리플로우를 최소화하자.
