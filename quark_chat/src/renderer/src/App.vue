<script setup>
import { ref, reactive } from 'vue'
import meAvatar from './assets/avatar.jpg?asset'
import youAvatar from './assets/avatar_2.webp?asset'
// 初始消息数据
const messages = reactive([
  {
    id: '327816731',
    role: 'me',
    avatar: meAvatar,
    text: 'Hello Lisa!'
  },
  {
    id: '727816761',
    role: 'you',
    avatar: youAvatar,
    text: 'Hello Jerry!'
  }
])
//获取表单中填写的内容(输入栏v-model="text",导入ref，使用ref，读取text.value)
const text = ref('')
var last_message = 'Hello Jerry!'
function sendMessage() {
  //console.log(text.value)
  //追加新的消息
  messages.push({
    id: Date.now(), //时间戳赋予id
    role: messages.length % 2 === 1 ? 'you' : 'me',
    avatar: messages.length % 2 === 1 ? youAvatar : meAvatar,
    text: text.value
  })
  // eslint-disable-next-line prettier/prettier
  last_message = messages.length % 2 === 1? text.value : "Lisa:"+text.value
  text.value = ''
}
</script>
<template>
  <div class="page-layout">
    <div class="layout-sidebar">
      <div class="user-profile">
        <img class="avatar" src="./assets/avatar.jpg" alt="" />
        <span class="status"></span>
      </div>
      <div class="menu-list">
        <a class="menu active" href="javascript:;">
          <span class="iconfont icon-message"></span>
        </a>
        <a class="menu" href="javascript:;">
          <span class="iconfont icon-user"></span>
        </a>
      </div>
    </div>
    <div class="layout-workbench">
      <div class="dragable layout-workbench-titlebar">
        <input class="input" type="text" />
        <span class="iconfont icon-plus"></span>
      </div>
      <div class="history-message">
        <div class="user-info">
          <img class="avatar" src="./assets/avatar.jpg" alt="" />
          <div class="extra">
            <span class="nickname">Lisa</span>
            <span class="message">{{ last_message }}</span>
          </div>
        </div>
      </div>
    </div>
    <div class="resize-line"></div>
    <div class="layout-main">
      <div class="dragable layout-main-titlebar">
        <span class="nickname">lotjol</span>
      </div>
      <div class="layout-main-messages">
        <div v-for="message in messages" :key="message.id" :class="['message', message.role]">
          <img class="avatar" :src="message.avatar" />
          <div class="text">{{ message.text }}</div>
        </div>
      </div>
      <div class="layout-main-toolkit">
        <!-- 输入框 打了回车做什么 -->
        <textarea v-model.trim="text" class="textarea" @keyup.enter="sendMessage"></textarea>
      </div>
    </div>
  </div>
</template>
<style lang="less">
@import './style.less';
</style>
