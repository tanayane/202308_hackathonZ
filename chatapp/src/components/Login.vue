<script setup>
import { inject, ref } from "vue"
import { useRouter } from "vue-router"
import io from "socket.io-client"

// #region global state
const userName = inject("userName")
// #endregion

// #region local variable
const router = useRouter()
const socket = io()
// #endregion

// #region reactive variable
const inputUserName = ref("")
// #endregion

// #region browser event handler
// 入室メッセージをクライアントに送信する
const onEnter = () => {
  // ユーザー名が入力されているかチェック
  if (inputUserName.value.length === 0) {
    alert("ユーザー名を入力してください")
    return false
  }

  // 入室メッセージを送信
  socket.emit("enterEvent", inputUserName.value + "さんが入室しました。")

  // 全体で使用するnameに入力されたユーザー名を格納
  userName.value = inputUserName.value

  // チャット画面へ遷移
  router.push({ name: "chat" })
}
// #endregion
</script>

<template>
  <div class="mx-auto my-5 px-4">
    <h1 class="text-h3 font-weight-medium">Vue.js Chat サンプル</h1>
    <div class="mt-10">
      <p>ユーザー名</p>
      <input type="text" v-model="inputUserName" class="user-name-text" />
    </div>
    <v-btn type="button" @click="onEnter" class="button-normal">入室する</v-btn>
  </div>
</template>

<style scoped>
.user-name-text {
  width: 200px;
  border: 1px solid #888;
  margin-bottom: 16px;
}
</style>
