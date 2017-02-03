<template>
<div>
    <h3>Chat</h3>
    <div>
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
      console.log(m)
      this.chat.push(m.data)
    }
  },
  methods: {
    chatMsg () {
      this.ws.send(this.msg)
      this.msg = ''
    }
  }
}

</script>
