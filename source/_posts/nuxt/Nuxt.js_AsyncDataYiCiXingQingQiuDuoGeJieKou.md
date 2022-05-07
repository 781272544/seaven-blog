title: nuxt.js性能提升 asyncData一次性请求多个接口, asyncData批处理请求
author: 68HTML
date: 2022-04-15 14:37:13
tags: ["Nuxt","asyncData","asyncData批处理"]
categories: ["Nuxt"]
---

## nuxt.js性能提升 asyncData一次性请求多个接口, asyncData批处理请求
```vue
<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">test_axios</h1>
      <div class="links">
        <a
          href="https://nuxtjs.org/"
          target="_blank"
          rel="noopener noreferrer"
          class="button--green"
        >
          Documentation
        </a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          rel="noopener noreferrer"
          class="button--grey"
        >
          GitHub
        </a>
      </div>
      <h1>新的</h1>
      <h3>IP：{{ip}}</h3>
      <h3>title:{{title}}</h3>
      <ul>
          <li v-for="(data,index) in list" :key="index">{{data.name}}+index:{{index}}</li>
      </ul>
    </div>
  </div>
</template>
 
<script>
export default {
  async asyncData ({ app }) {
    const $axios = app.$axios
    // 单个请求处理
    const data = await $axios.$get('http://192.168.1.181:8001/static/js/JsonData.json')
    const seo = data.head
    const list = data.list
    console.log(seo)
    console.log(list)
    // 多个请求批处理
    const [data1, data2, data3] = await Promise.all([
      $axios.$get('http://192.168.1.181:8001/static/js/JsonData.json'),
      $axios.$get('http://192.168.1.181:8001/static/js/JsonData2.json'),
      $axios.$get('http://192.168.1.181:8001/static/js/JsonData3.json')
    ])
    console.log(data1.head.title)
    console.log(data2.head.title)
    console.log(data3.head.title)
    return { seo, list }
  },
  data () {
    return {
      title: '首页',
      ip: '0.0.0.0',
      list: []
    }
  },
  head () {
    return {
      title: this.seo.title + '_' + this.title,
      meta: [
        { hid: 'keywords', name: 'keywords', content: this.seo.keywords },
        { hid: 'description', name: 'description', content: this.seo.description }
      ]
    }
  }
}
</script>
 
<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
 
.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}
 
.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}
 
.links {
  padding-top: 15px;
}
</style>
```
