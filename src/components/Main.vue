<template>
  <div>
    <h2 v-show="firstView">请输入关键字搜索</h2>
    <h2 v-show="loading">请求中...</h2>
    <h2 v-show="errorMsg">{{errorMsg}}</h2>
    <div class="row">
      {{datas}}
    </div>
  </div>
</template>

<script>
  import PubSub from 'pubsub-js'
  import axios from 'axios'

  export default {
    data () {
      return {
        firstView: true, // 是否显示初始页面
        loading: false, // 是否正在请求中
        datas: '', // 数据
        errorMsg: ''  //错误信息
      }
    },

    mounted () {
      // 订阅消息(search)
      PubSub.subscribe('search', (message, username) => { // 点击了搜索, 发ajax请求进行搜索

        // 更新数据(请求中)
        this.firstView = false
        this.loading = true
        this.datas = ''
        this.errorMsg = ''

        // 发ajax请求进行搜索
        const url = `http://192.168.3.38:8080/login?username=${username}`
        axios.post(url).then(response => {
            // 成功了, 更新数据(成功)
            this.loading = false
            this.datas = response.data
          })
          .catch(error => {
            // 失败了, 更新数据(失败)
            this.loading = false
            this.errorMsg = '请求失败!'
          })

      })
    }
  }
</script>

<style>
  .card {
    float: left;
    width: 33.333%;
    padding: .75rem;
    margin-bottom: 2rem;
    border: 1px solid #efefef;
    text-align: center;
  }

  .card > img {
    margin-bottom: .75rem;
    border-radius: 100px;
  }

  .card-text {
    font-size: 85%;
  }
</style>
