# 安装依赖
npm install

# 小程序运行
npm run dev

# H5运行
npm run devH5

# 小程序打包（打包到dist目录)
npm run build

# H5打包（打包到distH5目录)
npm run buildH5


## 开始
这个项目引用参考
>github源码 => https://github.com/Aimee1608/mpvuedemo



## 简易说明
### 路由跳转
<template>
  <div class="container">
      <!-- 图片引用的两种方式 -->
      <img class="girl" :src="imgSrc +'static/img/girl.png'" alt="">
      <img class="logo" src="../../assets/logo.png" alt="">
    <card :text="motto"></card>
    <form class="form-container">
      <input type="text" class="form-control" v-model="motto" placeholder="v-model" />
      <input type="text" class="form-control" v-model.lazy="motto" placeholder="v-model.lazy" />
    </form>
    <!-- 路由跳转 -->
    <a @click="gotoGame('pages/counter/main')" class="counter">去往Vuex示例页面</a>
    <a @click="gotoGame('pages/logs/main')" class="counter">去往logs页面</a>
  </div>
</template>

<script>
// 组件引用
import card from '@/components/card'

export default {
  data () {
    return {
      motto: 'Hello World'
    }
  },
  components: {
    card
  },
  methods: {
     gotoGame (path) {
        this.reLaunchPageTo(this.router + path)
     }
  }
}
</script>

### 分别封装方法
> H5 mainH5.js方法

Vue.mixin({
  data () {
    return {
      service: '', // 服务
      router: '/', // 路由路径
      imgSrc: '' // 图片路径
    }
  },
  methods: {
      newroot () {
          return  this.$route
      },
      navigatePageTo (url) {
          this.$router.push(url)
      },
      reLaunchPageTo (url) {
          this.$router.replace(url)
      },
      setStorageSync (name, data) {
          sessionStorage.setItem(name, JSON.stringify(data))
      },
      getStorageSync (name) {
          return JSON.parse(sessionStorage.getItem(name))
      }
  },
  created () {
      console.log('chrome')
      this.service = httpService
  }
})

> 小程序main.js

Vue.mixin({
  data() {
    return {
      service: '',
      router: '/',
      imgSrc: '/'
    }
  },
  methods: {
      newroot () {
          return this.$root.$mp
      },
      navigatePageTo (url) {
          wx.navigateTo({url: url})
      },
      reLaunchPageTo (url) {
          wx.reLaunch({url: url})
      },
      setStorageSync (name, data) {
          wx.setStorageSync(name, data)
      },
      getStorageSync (name) {
          return wx.getStorageSync(name)
      }
  },
  created() {
      this.service = wxService
  }
})
```

### vuex 数据存储
>小程序store 直接挂在 Vue原型上
Vue.prototype.$store = store

>H5vue 还是和之前一样
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>',
  store
})
