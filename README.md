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
