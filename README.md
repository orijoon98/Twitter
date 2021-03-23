# Twitter

## 플랫폼
- Firebase
- 아이디어를 구상하는 단계나 테스트 용도로 활용하자.

## 개발언어
- React

## Firebase Setup
- Firebase 에서 프로젝트를 생성한다.
- `npm install --save firebase` 명령어를 실행하여 npm 패키지를 설치하고 `package.json` 파일에 저장한다.
- Firebase 를 `import` 한다.
```
import firebase from "firebase/app";
```
- 앱에서  Firebase를 초기화한다.
```
const firebaseConfig = {
  // ...
};
// Initialize Firebase
firebase.initializeApp(firebaseConfig);
```

## Route 생성
- `npm install --save react-router-dom`
- 로그인이 안 된 상태면 `Auth.js` 파일로 `route`.
- 로그인이 된 상태면 `Home.js` 파일로 `route`.


## 로그인/로그아웃 구현
- firebase 파일에서 `firebase.auth()` 를 초기화해서 firebase의 `auth` 모듈에 접근한다.
```
export const authService = firebase.auth();
```
- 사용하고자 하는 파일에서 `import` 해준다.
```
import { authService } from "fbase";
```
- `authService.currentuser` 값이 `null` 이면 로그인이 안 된 상태를 나타낸다.
- Firebase 콘솔 - 빌드 - Authentication 에서 로그인 방법을 직접 설정해준다.
- 회원가입이 안 돼있으면 회원가입 후 로그인을 해주는 `createUserWithEmailAndPassword` 메소드 사용
- 회원가입이 돼있으면 로그인을 해주는 `signInWithEmailAndPassword` 메소드 사용
- 유저상태에 변화가 있을 때 알아차릴 수 있는 `onAuthStateChanged` 메소드 사용
- 주의 : firebase가 초기화되는데 시간이 걸리기 때문에 useEffect() 메소드 사용
- 다른 이메일 계정 사용할 때 `signInWithPopup` 메소드 사용
- 로그아웃 메소드 `signOut`을 사용하여 로그아웃

## Creating Tweet
- Firebase 콘솔에서 `Cloud Firestore` 즉 `데이터베이스`를 생성한다.
- `NoSQL`방식 : `Collection`은 `Document`의 그룹이다.
- firebase 파일에서 `firebase.firestore()` 를 초기화해서 firebase의 `firestore` 모듈에 접근한다.
```
export const dbService = firebase.firestore();
```
- 사용하고자 하는 파일에서 `import` 해준다.
```
import { dbService } from "fbase";
```
- `collection` 메소드를 사용하여 `firestore`의 `collection`에 데이터를 추가한다.
```
const onSubmit = async (event) => {
  event.preventDefault();
  await dbService.collection("tweets").add({
    tweet,
    createdAt: Date.now()
  });
  setTweet("");
};
```

## Reading Tweet
- `collection.get()` 메소드를 사용하게 되면 `QuerySnapshot` 을 반환하는데 우리가 원하는 `data`를 얻기 위해선 아래와 같은 작업을 해준다.
- 하지만 이 방법은 새로운 데이터를 얻기 위해선 새로고침을 해야한다는 단점이 있다.
```
const getTweets = async () => {
  const dbTweets = await dbService.collection("tweets").get();
  dbTweets.forEach(document => {
    const tweetObject = {
      ...document.data(),
      id: document.id
    };
    setTweets((prev) => [tweetObject, ...prev])
  });
};
```
- 실시간으로 `tweets` 컬렉션의 `data`를 얻기 위해선 `collection.onSnapshot` 메소드를 사용한다.
```
useEffect(() => {
  dbService.collection("tweets").onSnapshot(snapshot => {
    const tweetArray = snapshot.docs.map(doc => ({
      id:doc.id,
      ...doc.data()}));
      setTweets(tweetArray);
    });
}, []);
```

## Updating Tweet
- `id` 비교를 통해 `tweet` 의 작성자와 로그인한 사용자가 같을 때만 수정 버튼이 보이게 한다.
- 수정 버튼을 누르면 `update` 메소드를 통해 트윗을 수정한다.
```
const onSubmit = async (event) => {
  event.preventDefault();
  await dbService.doc(`tweets/${tweetObj.id}`).update({
    text:newTweet
  });
  setEditing(false);
};
```
- 트윗을 수정하다가 취소할 경우 기존에 있던 트윗 내용으로 돌아간다.
```
const toggleEditing = () => setEditing((prev) => !prev);
```

## Deleting Tweet
- `id` 비교를 통해 `tweet` 의 작성자와 로그인한 사용자가 같을 때만 삭제 버튼이 보이게 한다.
- `window.confirm()` 메소드를 통해 팝업창으로 예를 누르면 삭제 기능이 작동하게 한다.
- `delete()` 메소드를 사용해서 간편하게 삭제 기능을 구현할 수 있는데 이 때 경로를 지정해 준다.
```
const onDeleteClick = async () => {
  const ok = window.confirm("Are you sure you want to delete this tweet?");
  if(ok){
    await dbService.doc(`tweets/${tweetObj.id}`).delete();
  }
};
```