<template>
  <div class="about">
    <h1>This is an about page</h1>
    <button @click='sen'></button>
    <button @click='clo'>断开连接</button>
  </div>
</template>
<script>
export default {
  data () {
    return {
      ws: ''
    }
  },
  methods: {
    sen () {
      this.ws.send('测试websocket')
    },
    clo () {
      this.ws.close()
    },
    init () {
      if (window.WebSocket) {
        this.ws = new WebSocket('ws://localhost:8001')
        var ws = this.ws
        ws.onopen = (e) => {
          console.log('连接服务器成功')
          // ws.send('what`s your name?')
        }
        ws.onclose = function (e) {
          console.log('服务器关闭')
        }
        ws.onerror = function () {
          console.log('连接出错')
        }
        // 接收服务器的消息
        ws.onmessage = function (e) {
          let message = 'message:' + e.data + ''
          console.log(message)
        }
      } else {
        alert('浏览器不支持socket')
      }
    }
  },
  created () {
    this.init()
  },
  /* destroyed () {
    this.ws.onclose = (e) => {
      console.log('服务器关闭')
    }
  }, */
  beforeRouteLeave (to, from, next) {
    this.ws.close()
    next()
  }
}
</script>
