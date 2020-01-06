<template>
  <section class="jumbotron">
    <h3 class="jumbotron-heading">Search Github Users</h3>
    <div>
      <input type="text" placeholder="enter the name you search" v-model="username"/>
      <button @click="search">Search</button>
    </div>
  </section>
</template>

<script>
  import PubSub from 'pubsub-js'
  export default {
    data () {
      return {
        username: ''
      }
    },

    methods: {
      search () {
        const username = this.username.trim()
        if(username) {
          // 分发一个search的消息
          PubSub.publish('search', username)
        }
      }
    }
  }
</script>

<style>

</style>
