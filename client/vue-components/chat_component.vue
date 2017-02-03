<template>
<div>
    <h3>Chat</h3>
    <div id="chatbox">
      <div v-for="msg in chat"> {{ msg }}</div>
    </div>
    <input v-model="msg" @keyup.enter="chatMsg" type="text" name="" value="" />
</div>
</template>
<script>
export default {
  data() {
    return {
      msg: '',
      chat: []
    }
  },
  props: ['ws'],
  mounted() {
    this.ws.onmessage = (m) => {
      this.chat.push(m.data)
    }
  },
  methods: {
    chatMsg () {
      this.ws.send(this.msg)
      this.msg = ''
      let ch =document.getElementById("chatbox")
      ch.scrollTop = ch.scrollHeight - ch.clientHeight
    }
  }
}

</script>

<style>
#chatbox{
  height: 30vh;
  width: auto;
  overflow: auto;
}
input{
  background-color: white;
}
</style>
