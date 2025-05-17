## CJS (CommonJS)
- 별도 설정이 없다면 CJS가 기본값 
```javascript
function add(a, b){
  return a + b;
}

function sub(a, b){
  return a - b;
}

module.exports = {
  add,
  sub,
}
```
```javascript
const cal = require("./math");
const {add, sub} = require("./math");

console.log(cal.add(1, 2)); //3
console.log(sub(1, 2)); //-1
```
<br/>

## ESM (ES Modules)
- `package.json`에서 `"type": "module"` 설정 필요
```javascript
function add(a, b){
  return a + b;
}

function sub(a, b){
  return a - b;
}

export default multiply(a, b){
  return a * b;
}

export {add, sub}
```
```javascript
// ⚠️ 파일 확장자 꼭 명시!
import multiply, {add, sub} from "./math.js"

console.log(add(1, 2)); //3
console.log(sub(1, 2)); //-1
```
