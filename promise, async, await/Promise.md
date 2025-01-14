# Promise
- 비동기 작업을 처리하는데 사용하는 객체
- 비동기 함수가 반환하는 객체
- 비동기 함수의 결과를 담고 있는 객체
	- 어떤 작업을 처리하고 결과를 Promise 객체를 통해 받음
- resolve: 정상적인 결과 값을 반환 (이행) 했을 때 실행되는 콜백 함수
- reject: 정상적이지 않은 결과 값을 반환 (거부) 했을 때 실행되는 콜백 함수
```javascript
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    const text = prompt('Hello!');
    if (text === 'hello') {
      resolve('💻')
    } else {
      reject('error message');
    }
  }, 2000)
})

myPromise
  .then(res => console.log(`res: ${res}`))
  .catch(err => console.log(`err: ${err}`))
  .finally(() => console.log('###finally###'))
```
- Promise 객체의 상태
  - 비동기 작업이 진행 중인 상태: 대기(pending)
  - 비동기 작업이 정상적으로 처리된 상태: 이행(fulfilled)
  - 비동기 작업이 정상적으로 처리되지 않은 상태: 거부(rejected)
- Promise 메소드
	- then(): 이행되었을 때
	- catch(): 거부되었을 때
	- finally(): 이행되거나 거부되더라도 항상
- Promise 객체는 생성된 즉시 비동기 작업 수행

# Promise chaining
```javascript
fetch('https://jsonplaceholder.typicode.com/todos?_limit=5')
  .then((response) => {
    return response.json()
  })
  .then(data => {
    console.log(`data: ${data}`)
    return data.filter(obj => obj.id > 3)
  })
  .then((result) => {
    console.log(`result: ${result}`);
  })
  .catch((err) => {
    console.log(`err: ${err}`)
  })
  .finally(() => {
    console.log('finally');
  })
```
- 콜백 지옥 개선
```javascript
function getUser(userId){
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      try{
        const user = {id: userId, name: 'coding'};
        resolve(user);
      } catch (error) {
        reject(error)
      }
    }, 1000)
  })
}

function getPosts(userId){
  return new Promise((resolve) => {
    setTimeout(() => {
      const posts = [
        {id: 1, title: 'Post 1'},
        {id: 2, title: 'Post 2'},
      ];
      resolve(posts[userId-1])
    }, 1000)
  })
}

function getComments(userId){
  return new Promise((resolve) => {
    setTimeout(() => {
      const comments = [
				{id: 1, text: 'Comment 1'},
				{id: 2, text: 'Comment 2'},
				{id: 3, text: 'Comment 3'},
      ]
      resolve(comments[userId-1].text);
    })
  }, 1000)
}

getUser(1)
  .then(user => {
    console.log(`user: ${user}`);
    return getPosts(user.id)
  })
  .then(posts => {
    console.log(`posts: ${posts}`);
    return getComments(posts.id);
  })
  .then(comments => {
    console.log(`comments: ${comments}`)
  })
  .catch(err => console.log(`err: ${err}`))
  .finally(() => console.log('###finally###'))
```

# Promise static method
- Promise.resolve(): 주어진 값을 가지고 이행(fulfilled)되는 프로미스 객체를 생성하는 역할
- Promise.reject(): 주어진 값을 가지고 거부(rejected)되는 프로미스 객체를 생성하는 역할
- Promise.all(): 여러 개의 프로미스를 동시에 실행할 때 사용하는 메서드
- Promise.allSettled(): 여러 개의 프로미스를 동시에 실행하고 모든 프로미스가 이행되거나 거부될 때 까지 기다림
- Promise.any(): 여러 개의 프로미스를 동시에 실행하고 하나라도 이행하면 해당 프로미스의 값을 반환, 모든 프로미스가 거부되는 경우에만 전체 프로미스가 거부됨
- Promise.race(): 가장 먼저 처리된 프로미스가 이행되던 거부되던 그 값을 반환
