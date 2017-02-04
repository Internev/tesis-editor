<template>
<div class="bored">
    <h3>Chat</h3>
    <div id="chatbox">
      <div v-for="msg in chat"> {{ msg }}</div>
    </div>
    <div class="textbox row">
      <span class="glyphicon glyphicon-user col-sm-1 usericon"></span>
      <input class="col-sm-9" v-model="msg" @keyup.enter="chatMsg" type="text" name="" value="" />
      <span class="glyphicon glyphicon-send col-sm-1"></span>
    </div>
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
  props: ['ws', 'name'],
  mounted() {
    this.ws.onmessage = (m) => {
      m = JSON.parse(m.data)
      this.chat.push(`${m.name}: ${m.msg}`)
    }
  },
  methods: {
    chatMsg () {
      let msgObj = {
        name: this.name,
        msg: this.msg
      }
      this.ws.send(JSON.stringify(msgObj))
      this.msg = ''
      // handle scrolling to bottom of div if longer than height.
      let ch =document.getElementById("chatbox")
      ch.scrollTop = ch.scrollHeight - ch.clientHeight
    }
  }
}

</script>

<style>
.bored{
  width: 95%;
  border: 2px solid pink;
  padding: 2%
}
#chatbox{
  height: 40vh;
  width: auto;
  overflow: auto;
}
.textbox{
  text-align: left;
  width: 100%
}
.glyphicon{
  cursor: pointer;
  color: #000;
  font-size: 1.5em;
}
.usericon{
    margin-right: .7em;
}
</style>
