## event bubbling
- 한 요소에 이벤트가 발생하면...
  - 1️⃣ 요소에 할당된 핸들러가 동작
  - 2️⃣ 부모 요소의 핸들러가 동작
  - 3️⃣ 최상단의 조상 요소를 만날 때까지 이 과정이 반복
```html
<style>
  body * {
    margin: 10px;
    border: 1px solid blue;
  }
</style>

<form onclick="alert('form')">FORM
  <div onclick="alert('div')">DIV
    <p onclick="alert('p')">P</p>
  </div>
</form>
```
  - 1️⃣ p의 onclick 핸들러가 동작: alert("p");
  - 2️⃣ div의 onclick 핸들러가 동작: alert("div");
  - 3️⃣ form의 onclick 핸들러가 동작: alert("form");
  - 4️⃣ document 객체를 만날 때까지 각 요소에 할당된 핸들러가 동작
<br/>

## `event.target`
- `event.target`
  - 실제 이벤트가 시작된 타깃 요소 (이벤트가 발생한 가장 안쪽의 요소)
  - 버블링이 진행되어도 변하지 않음
- `this` (=`event.currentTarget`)
  - 현재 요소
  - 현재 실행 중인 핸들러가 할당된 요소를 참조
```html
<!DOCTYPE HTML>
<html>

<head>
  <meta charset="utf-8">
  <link rel="stylesheet" href="example.css">
  <style>
    form {
      background-color: green;
      position: relative;
      width: 150px;
      height: 150px;
      text-align: center;
      cursor: pointer;
    }
    
    div {
      background-color: blue;
      position: absolute;
      top: 25px;
      left: 25px;
      width: 100px;
      height: 100px;
    }
    
    p {
      background-color: red;
      position: absolute;
      top: 25px;
      left: 25px;
      width: 50px;
      height: 50px;
      line-height: 50px;
      margin: 0;
    }
    
    body {
      line-height: 25px;
      font-size: 16px;
    }
  </style>
</head>

<body>
  클릭하면 <code>event.target</code>과 <code>this</code>정보를 볼 수 있습니다.
  <form id="form">FORM
    <div>DIV
      <p>P</p>
    </div>
  </form>
  <script>
    form.onclick = function(event) {
      event.target.style.backgroundColor = 'yellow';
      
      // chrome needs some time to paint yellow
      setTimeout(() => {
        alert("target = " + event.target.tagName + ", this=" + this.tagName);
        event.target.style.backgroundColor = ''
      }, 0);
    };
  </script>
</body>
</html>
```
- `event.target`은 실제 클릭한 요소, `this(event.currentTarget)`은 항상 `form`
<br/>

## 버블링 중단하기
- `event.stopPropagation()`: 버블링을 막아주지만, 동일한 요소의 이벤트를 처리하는 다른 핸들러의 동작을 막아주지는 못함
- `event.stopimmediatePropagation()`: 버블링을 막아주고, 요소에 할당된 다른 핸들러의 동작도 막아줌
- ⚠️ 꼭 필요한 경우가 아니라면 버블링을 막지 말기!
```html
<body onclick="alert(`버블링은 여기까지 도달하지 못합니다.`)">
  <button onclick="event.stopPropagation()">클릭해 주세요.</button>
</body>
```
