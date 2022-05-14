<template>
  <section>
    <div class="alert h6 text-right" @click="doLogin">
      [login:{{ data.user != null ? data.user.displayName : "---" }}]
    </div>
    <h2>{{ title }}</h2>
    <p class="h5">{{ data.message }}</p>
    <div v-if="data.user" class="alert alert-primary">
      <div class="form-group text-left">
        <label class="h5">Message</label>
        <div class="form-row">
          <div class="col-10">
            <input v-model="data.msg" class="form-control" />
          </div>
          <button @click="add" class="btn btn-primary col-2">投稿</button>
        </div>
      </div>
      <h3 class="my-3">Messages</h3>
      <ul v-for="(item, key) in data.fire_data" :key="key" class="list-group text-left">
        <li class="list-group-item">
          <div class="h5">{{ item.msg }}</div>
          <div class="small text-right">
            {{ item.user }} ({{ item.posted }})
          </div>
        </li>
      </ul>
    </div>
    <div v-else>
      <div class="alert alert-warning">※現在、ログインされていません。</div>
    </div>
  </section>
</template>

<script lang="tsx">
import { onMounted, reactive } from "vue";
import { initializeApp } from 'firebase/app';
import { getAuth, signInWithPopup, GoogleAuthProvider } from "firebase/auth";
import { getDatabase, ref, set, query, orderByKey, limitToLast, onValue } from "firebase/database";

const firebaseConfig = {
  apiKey: "AIzaSyBBqtmACsNXEew4EWtMuy1F6nbJPnplq6s",
  authDomain: "api-project-842853325730.firebaseapp.com",
  databaseURL: "https://api-project-842853325730-default-rtdb.firebaseio.com",
  projectId: "api-project-842853325730",
  storageBucket: "api-project-842853325730.appspot.com",
  messagingSenderId: "842853325730",
  appId: "1:842853325730:web:4643506f259a8293a7f0fb",
};

const app = initializeApp(firebaseConfig);
const provider = new GoogleAuthProvider();
const auth = getAuth();
const db = getDatabase(app);

const person = ref(db, 'board');

export default {
  setup(props: any) {
    const data = reactive({
      title: "Board",
      message: "ミニ伝言板。最新の投稿を表示します。",
      user: "",
      msg: "",
      num_per_page: 10, //☆取り出すデータ数
      fire_data: {},
    });
    // ログイン処理
    const login = () => {
      signInWithPopup(auth, provider)
        .then((result) => {
          data.user = result.user.email ? result.user.email : "";
          data.message = "ログインしました。";
          console.log("ログインしました");
          let board = query(ref(db, 'board'), orderByKey(), limitToLast(data.num_per_page))
          onValue(board, (snapshot) => {
            console.log("firebase on");
            if (auth.currentUser != null) {
              let arr = [];
              let result = snapshot.val();
              for (let item in result) {
                arr.unshift(result[item]);
              }
              console.log(arr);
              data.fire_data = arr;
            } else {
              data.fire_data = {};
            }
          })
          /*
          firebase
            .database()
            .ref("board")
            .orderByKey()
            .limitToLast(data.num_per_page)
            .on("value", (snapshot) => {
              console.log("firebase on");
              if (auth.currentUser != null) {
                let arr = [];
                let result = snapshot.val();
                for (let item in result) {
                  arr.unshift(result[item]);
                }
                console.log(arr);
                data.fire_data = arr;
              } else {
                data.fire_data = {};
              }
            });
            */
        });
    };
    // ログアウト処理
    const logout = () => {
      auth.signOut();
      data.user = "";
      data.fire_data = {};
      data.message = "ログアウトしました。";
    };
    // ログイン・ログアウト実行
    const doLogin = () => {
      if (auth.currentUser == null) {
        login();
      } else {
        logout();
      }
    };
    // メッセージ追加
    const add = () => {
      if (auth.currentUser == null) {
        alert("ログインしないと投稿できません。");
        return;
      }
      let user = auth.currentUser;
      console.log(user);
      let d = new Date();
      let dstr =
        d.getFullYear() +
        "-" +
        (d.getMonth() + 1) +
        "-" +
        d.getDate() +
        " " +
        d.getHours() +
        ":" +
        d.getMinutes() +
        ":" +
        d.getSeconds();
      let id = d.getTime();
      set(ref(db, 'board/' + id), {
        msg: data.msg,
        user: user.displayName,
        posted: dstr,
      });
      data.msg = "";
      data.message = "投稿しました。";
      console.log("投稿しました。");
      /*
      firebase
        .database()
        .ref("board/" + id)
        .set(obj);
      data.msg = "";
      data.message = "投稿しました。";
      console.log("投稿しました。");
      */
    };
    onMounted(() => {
      if (auth.currentUser == null) {
        login();
      }
      console.log(auth.currentUser);
    });
    return { data, login, logout, doLogin, add };
  },
};
</script>