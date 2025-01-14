```javascript
function taskAsyncFunction(callback){
	console.log('start');
	setTimeout(() => {
		console.log('첫 번째 작업');
		console.log('첫 번째 작업');
		callback();
	}, 2000)
	console.log('end');
}

taskSyncFunction(() => {
	console.log('콜백 함수 실행')
})

console.log('실행 완료!');
```

- callback hell
```javascript
function getUser(userId, callback){
  setTimeout(() => {
    const user = {id: userId, name: 'code'};
    callback(user);
  }, 1000)
}

function getPosts(userId, callback){
  setTimeout(() => {
    const posts = [
      {id: 1, title: 'Post 1'},
      {id: 2, title: 'Post 2'},
    ]
    callback(posts);
  }, 1000)
}

function getComments(postId, callback){
  setTimeout(() => {
    const comments = [
      {id: 1, text: 'Comment 1'},
			{id: 2, text: 'Comment 2'},
			{id: 3, text: 'Comment 3'},
    ]
    callback(comments);
  }, 1000)
}

getUser(1, user => {
  console.log(`user: ${user}`);
  getPosts(user.id, posts => {
    console.log(`posts: ${posts}`)
    getComments(posts[0].id, comments => {
      console.log(`comments: ${comments}`)
    })
  })
})
```
- 콜백 지옥 해결책: Promise, async, await
