- 비동기 함수를 동기 방식처럼 작성할 수 있도록 도와줌
- async
  - 함수 앞에 async를 붙이면 Promise를 반환하는 비동기적인 함수를 선언하는 것
  - 비동기 함수는 항상 Promise 반환
  - 함수 내부에서 일반 값을 반환해도 자동으로 Promise로 감싸서 반환
- await
  - async 함수 안에서만 사용 가능
  - Promise가 처리될 때까지 함수의 실행을 잠시 멈춤
  - 사실상 then과 같은 역할
  - await를 사용하면 콜백 함수나 promise chaining 대신 동기 방식으로 코드를 작성할 수 있어 가독성 향상
- async와 await는 비동기 코드를 마치 동기 코드 처럼 읽히게 만들어주는 문법이지만 전체 코드가 진짜 동기적으로 동작하는 것은 아님 
	- await 뒤의 Promise가 해결될 때까지 해당 async 함수 내의 실행은 멈춤 
	- await는 이벤트 루프를 블록하지 않음. await 때문에 다른 코드가 실행되는 것을 막지 않음
	- 함수 외부의 코드나 다른 async 함수 호출은 여전히 비동기적으로 처리됨

```javascript
async function getUserData(userId){
  try{
    const response = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);
    if (!response.ok) {
      throw new Error('네트워크 응답에 문제가 있습니다.')
    }

    const userData = await response.json();
    return userData;
  } catch (error) {
    console.log(`데이터를 가져오는 중 오류 발생: ${error}`)
  }
}

async function displayUser() {
  const user = await getUserData(1);
  console.log(user);
}

displayUser();
```
