# 자바스크립트 팁 정리
## arguments 를 활용
### 파라미터가 유동 적일 때 arguments를 활용할 수 있다.
```javascript
function sum() {
  var result = 0;
  for(var i=0,j=arguments.length; i<j; i++){
    result += arguments[i];
  }
  return result;
}

console.log(sum(1,2,3,4));
```
## 상속
### 자바스크립트도 자바와 유사하게 상속을 사용 할 수 있다.
#### java
```java
class A {
	private String nation = "korea";

	public void getNation() {
		System.out.println(nation);
	}
}

class B extends A { }

public class ExtendTest {
	public static void main(String[] args) {
		B b = new B();
		b.getNation();
	}
}
```
#### javascript
```javascript
function A() {
  this.nation = "korea";
}
A.prototype.getNation = function() {
  console.log("My Nation : " + this.nation);
}

function B() { }
B.prototype = new A();

var b = new B();
b.getNation();
```
## 생성자
### 초기 설정 개체의 prototype 에는 constructor 속성을 포함한다.
+ constructor 속성은 초기 설정을 가지는 모든 초기 설정 개체의 멤버입니다. 여기에는 Global 및 Math 개체를 제외한 모든 내장 JavaScript 개체가 포함됩니다. constructor 속성은 특정 개체의 인스턴스를 구성하는 함수에 대한 참조를 포함합니다.
+ 참조 (https://msdn.microsoft.com/ko-kr/library/c1hcx253(v=vs.94).aspx)
```javascript
// A constructor function.
function MyObj() {
    this.number = 1;
}

var x = new String("Hi");

if (x.constructor == String)
    document.write("Object is a String.");
document.write ("<br />");

var y = new MyObj;
if (y.constructor == MyObj)
    document.write("Object constructor is MyObj.");

// Output:
// Object is a String.
// Object constructor is MyObj.
```
### 자식 개체가 부모 개체를 상속하면 생성자에 부모 생성자 가 세팅 된다.
+ 자바스크립트는 자바와 달리 개체를 상속할때는 생성자를 주의 하여야 한다.
```javascript
function A() {
  this.nation = "korea";
}
A.prototype.getNation = function() {
  console.log("My Nation : " + this.nation);
}

function B() { }
B.prototype = new A();

var a = new A();
var b = new B();

console.log(a.constructor === A); // true
console.log(b.constructor === A); // true
console.log(b.constructor === B); // false

// 생성자에 자기 자신을 세팅 하자
B.prototype.constructor = B;

console.log(b.constructor === A); // false
console.log(b.constructor === B); // true
```
