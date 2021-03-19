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


## 로그인 구현
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
