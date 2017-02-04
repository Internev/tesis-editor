<template>
<div class="bored">
    <h3>Chat</h3>
    <div id="chatbox">
      <div v-for="msg in chat"> {{ msg }}</div>
    </div>
    <div class="user-select col-sm-12" v-show="userSelect">
      <div class="row">Set Name:</div>
      <div class="row">
      <input type="text" v-model="name" @keyup.enter="userSelect = false" class="col-sm-6"/>
      <span class="col-sm-1" @click="userSelect = false"><i class="glyphicon glyphicon-ok"></i></span>
      </div>
    </div>
    <div class="textbox row">
      <span class="col-sm-1 usericon" @click="userSelect = true"><i class="glyphicon glyphicon-user"></i></span>
      <input class="col-sm-9" v-model="msg" @keyup.enter="chatMsg" type="text" name="" value="" />
      <span class="col-sm-1" v-on:click="chatMsg"><i class="glyphicon glyphicon-send"></i></span>
    </div>
</div>
</template>
<script>
export default {
  data() {
    return {
      msg: '',
      chat: [],
      userSelect: false
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
    chatMsg() {
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
  /*border: 2px solid pink;*/
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
.user-select{
  margin: .5em;
  padding: .5em;
}
</style>
