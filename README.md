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

## 参考文献

- Vue.js
  - [Vue.js3超入門](https://www.shuwasystem.co.jp/book/9784798063737.html)

- Firebase
  - [Firebase 初期化](https://firebase.google.com/docs/web/setup?hl=ja)
  - [Google 認証](https://firebase.google.com/docs/auth/web/google-signin?hl=ja)
  - [Realtime Database 読み書き](https://firebase.google.com/docs/database/web/read-and-write?hl=ja)

  

## サーバー構築（CentOS Stream release 8）

> 動作環境
> - [WebARENA Indigo](https://web.arena.ne.jp/indigo/)
>   - 事前に以下で、Webサーバー環境を作っておく
>     - `01_Indigo.md`
>     - `02_Apache.md`
>     - `80_NodeJs.md`

### 動作環境構築

```
# git clone https://github.com/develop986/vite_vue_firebase
# git pull

# mkdir /var/www/vite_vue_firebase
# cp -a vite_vue_firebase/dist/* /var/www/vite_vue_firebase/
```

```
ビルドする場合は、更に以下を実施

# cd vite_vue_firebase
# npm install
$ npm run build
```

### VirtualHost 設定

```
# vi /etc/httpd/conf/httpd.conf

以下のファイルを作成する

<VirtualHost *:80>
    ServerAdmin room@mysv986.com
    DocumentRoot /var/www/vite_vue_firebase/
    ServerName vitevuefirebase.mysv986.com
    ServerAlias vitevuefirebase.mysv986.com
RewriteEngine on
RewriteCond %{SERVER_NAME} =vitevuefirebase.mysv986.com
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```

```
# systemctl restart httpd
# certbot --apache -d vitevuefirebase.mysv986.com
# systemctl restart httpd
```

```
# cat /etc/httpd/conf/httpd-le-ssl.conf

以下のファイルが作成される。

<VirtualHost *:443>
    ServerAdmin room@mysv986.com
    DocumentRoot /var/www/vite_vue_firebase/
    ServerName ngbootstrapsample.mysv986.com
    ServerAlias ngbootstrapsample.mysv986.com

SSLCertificateFile /etc/letsencrypt/live/ngbootstrapsample.mysv986.com/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/ngbootstrapsample.mysv986.com/privkey.pem
Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>
```

### 課題

- ポップアップブロック対策ができていない