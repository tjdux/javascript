## var 키워드로 선언한 변수의 문제점
### 변수 중복 선언 허용
- 의도치 않게 먼저 선언된 변수 값이 변경되는 부작용 
```javascript
var x = 1;
var y = 1;

// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작
var x = 100;
// 초기화문이 없는 변수 선언문은 무시
var y;

console.log(x); // 100
console.log(y); // 1
```
### 함수 레벨 스코프
- 함수의 코드 블록만을 지역 스코프로 인정
- 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수
- 의도치 않게 전역 변수가 중복 선언되는 경우 발생 
```javascript
// 1️⃣
var x = 1;

if (true){
  var x = 10;
}

console.log(x); //10

// 2️⃣
var i = 10;

for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

console.log(i); // 5
```
### 변수 호이스팅
- 변수 호이스팅에 의해 변수 선언문 이전에 참조 가능
- 단, 할당문 이전에 변수를 참조하면 undefined 반환
```javascript
console.log(foo); // undefined

foo = 123;

console.log(foo); // 123

var foo;
```
<br/>

## let 키워드
### 변수 중복 선언 금지
- 중복 선언하면 문법 에러 (SyntaxError) 발생
```javascript
let bar = 123;
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```
### 블록 레벨 스코프 
- 모든 코드 블록을 지역 스코프로 인정하는 블록 레벨 스코프를 따름
```javascript
// 1️⃣
let foo = 1;
{
  let foo = 2;
  let bar = 3;
}
console.log(foo); //1
console.log(bar); //ReferenceError: bar is not defined

// 2️⃣
let i = 0;

function foo(){
  let i = 100;

  for (let i = 0; i < 2; i++){
    console.log(i); // 0 1
  }

  console.log(i); // 100
}

foo();

console.log(i); // 0
```
### 변수 호이스팅
- 변수 호이스팅이 발생하지 않는 것처럼 동작
```javascript
console.log(foo); // ReferenceError: foo is not defined
let foo;
```
- var 키워드
  - 암묵적으로 선언 단계와 초기화 단계가 한번에 진행
  - 선언 단계에서 스코프 (실행 컨텍스트의 렉시컬 환경 Lexical Environment)에 변수 식별자를 등록해 자바스크립트 엔진에 변수의 존재를 알림
  - 즉시 초기화 단계에서 undefined으로 변수를 초기화
  - 변수 할당문에 도달하면 비로소 값이 할당
  ```javascript
  // 선언 단계와 초기화 단계가 동시에 진행 
  console.log(foo); // undefined
  
  var foo;
  console.log(foo); // undefined
  
  foo = 1; // 할당문에서 값을 할당 
  console.log(foo); // 1
  ```
- let 키워드
  - **선언 단계와 초기화 단계가 분리되어 진행**
  - 암묵적으로 선언 단계가 먼저 실행되고 초기화 단계는 변수 선언문에 도달했을 때 실행
  - 초기화 단계 이전에 변수에 접근하려고 하면 참조 에러 ReferenceError 발생
  - **TDZ (Temporal Dead Zone): 스코프 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간**
  ```javascript
  // 선언 단계만 진행
  // 초기화 이전의 TDZ에서는 변수를 참조 불가능 
  console.log(foo); // ReferenceError: foo is not defined
  
  let foo; // 변수 선언문에서 초기화 단계
  console.log(foo); // undefined
  
  foo = 1; // 할당문에서 값 할당 
  console.log(foo); // 1
  ```
- let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 보이지만 그렇지 않음
  ```javascript
  let foo = 1;

  {
    console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
    let foo = 2;
  }
  ```
  - 호이스팅이 발생하지 않는다면 전역 변수 foo (1)를 출력해야 함
  - 하지만 let 키워드로 선언한 변수도 호이스팅이 발생하기 때문에 참조 에러가 발생
- 모든 선언 (var, let const, function , function*, class 등)을 호이스팅
### 전역 객체와 let
- var: var 키워드로 선언한 전역 변수와 전역 함수, 암묵적 전역은 전역 객체의 프로퍼티가 됨
- let
  - let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님
  - 보이지 않는 개념적인 블록 (전역 렉시컬 환경의 선언적 환경 레코드) 내에 존재
```javascript
// 1️⃣ var

// 전역 변수
var x = 1;
// 암묵적 전역
y = 2;
// 전역 함수
function foo() {}

console.log(globalThis.x); // 1
console.log(globalThis.y); // 2
console.log(globalThis.foo); // ƒ foo() {}

// 2️⃣ let
let z = 1;

console.log(globalThis.z); //undefined
```
<br/>

## const 키워드
- 상수(constant)를 선언하기 위해 사용
- let 키워드의 특징과 대부분 동일
### 선언과 초기화
- **반드시 선언과 동시에 초기화**
```javascript
const foo; // SyntaxError: Missing initializer in const declaration
```
- 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작
```javascript
{
  // 변수 호이스팅이 발생하지 않는 것처럼 동작
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  const foo = 1;
  console.log(foo); // 1
}

// 블록 레벨 스코프
console.log(foo); // ReferenceError: foo is not defined
```
### 재할당 금지
```javascript
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```
### 상수
- **상수**
  - **재할당이 금지된 변수**
  - 상태 유지, 가독성, 유지보수의 편의를 위해 적극적으로 사용
  - 대문자, 스네이크 케이스로 표현하는 것이 일반적
- **const 키워드로 선언된 변수에 원시 값을 할당한 경우, const 키워드에 의해 재할당이 금지되고 원시 값은 변경 불가능한 값이므로 할당된 값을 변경할 수 없음**
```javascript
// 세율
const TAX_RATE = 0.1;

// 세전 가격
let preTaxPrice = 100;

// 세후 가격
let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

console.log(afterTaxPrice); // 110
```
### const 키워드와 객체
- **const 키워드로 선언된 변수에 객체를 할당한 경우 값 변경 가능**
- 객체는 변경 가능한 값이기 때문에 재할당 없이도 직접 변경 가능
- **const 키워드는 재할당을 금지할 뿐 "불변"을 의미하지는 않음**
```javascript
const person = {
  name: 'Lee'
};

person.name = 'Kim';

console.log(person); // {name: "Kim"}
```
<br/>

## var vs. let vs. const
- 기본적으로 const를 사용하고 재할당이 필요한 경우 let으로 변경
  - 재할당이 필요한 경우에만 let 사용. 변수의 스코프는 최대한 좁게!
  - 재할당이 필요 없는 상수 원시 값과 객체는 cosnt 사용. const가 var, let 키워드보다 안전
- ES6를 사용한다면 var 키워드는 사용하지 말 것
