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
