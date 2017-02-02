<template>
<div>
    <h3>Chat</h3>
    <div>
      {{ msg }}
    </div>
    <input v-model="msg" type="text" name="" value="" />
</div>
</template>
<script>
export default {
  data() {
    return {
      msg: ''
    }
  }
}

</script>
