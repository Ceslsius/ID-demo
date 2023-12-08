<script setup>
import { RouterLink, RouterView, stringifyQuery } from 'vue-router'
import axios from "axios";
import HelloWorld from './components/HelloWorld.vue'
import {ref} from 'vue'
const info = ref(null)
const openDevice = () => {
  axios.get("http://localhost:19196/openDevice").then(res => {
    console.log(res)
    if (res.data.resultFlag == 0) {
      console.log('建立链接成功')
    }
  })
}
const readCard = () => {
  axios.get("http://localhost:19196/readCard").then((res) => {
    if (res.data.resultFlag == 0) {
      const obj = res.data
      // 处理身份证照片
      obj.identityPic = 'data:image/jpeg;base64,' + obj.identityPic
      // 处理身份证类型
      if (obj.certType == " ") {
        obj.certType = "身份证";
      } else if (obj.certType == "I") {
        obj.certType = "外国人居住证";
      } else if (obj.certType == "J") {
        obj.certType = "港澳台居住证";
      } else {
        obj.certType = "未知";
      }
      info.value = obj
      console.log(info.value)
      console.log(obj)
    } else {
      console.log('读卡失败')
    }
  })
}
const CloseDevice = () => {
  axios.get("http://localhost:19196/CloseDevice")
}
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />
    <div class="btn-group">
      <div @click="openDevice">建立链接</div>
      <div @click="readCard">读取信息</div>
      <div @click="CloseDevice">关闭连接</div>
    </div>
    <img :src="info.identityPic" v-if="info"/>
    <div class="">{{ info ? JSON.stringify(info) : '' }}</div>
  </header>

  <!-- <RouterView /> -->
</template>

<style scoped>
.btn-group{
  width: 100%;
  height: 40px;
  display: flex;
}
.btn-group div{
  height: 40px;
  line-height: 40px;
  padding: 0 20px;
  margin-left: 30px;
  background: #ccc;
}
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
