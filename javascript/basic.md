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
