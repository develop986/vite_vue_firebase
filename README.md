# Vite、Vue.js、Firebase サンプルページ

> Vue.Js と Firebase を使ったサンプルページです
> - Firebase Authentication を使ったGoogle認証
> - Firebase Realtime Database を使った、メッセージ保存機能

## プロジェクト作成方法

Vue.js、TypeScript を指定し、雛形プロジェクトを作成する

```
$ npm init vite@latest
```

> Need to install the following packages:  
>   create-vite@latest  
> Ok to proceed? (y) y  
> ✔ Project name: … vite_vue_firebase  
> ✔ Select a framework: › vue  
> ✔ Select a variant: › vue-ts  

```
$ npm install firebase
$ npm install bootstrap-vue
```

## 実行方法

```
$ npm install
$ npm run dev
```

- http://localhost:3000/

## ビルド

```
$ npm run build
$ npm run preview
```

- http://localhost:4173/

または、http-server を使用する場合

```
$ sudo npm install -g http-server
$ http-server ./dist/ -p3000 -c-1
```

- http://localhost:3000/

## サーバー構築

> 準備中

## 参考文献

- Vue.js
  - [Vue.js3超入門](https://www.shuwasystem.co.jp/book/9784798063737.html)

- Firebase
  - [Firebase 初期化](https://firebase.google.com/docs/web/setup?hl=ja)
  - [Google 認証](https://firebase.google.com/docs/auth/web/google-signin?hl=ja)
  - [Realtime Database 読み書き](https://firebase.google.com/docs/database/web/read-and-write?hl=ja)