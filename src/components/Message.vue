<template>
  <div class="message-container" v-html="msg"></div>
</template>

<script>
export default {
  name: "Message",
  props: {
    msg: String
  },
  methods: {
    //Msg pedido deletado, msg do sistema
    showMessage(message) {
      this.msg = message;
    }
  }
};
</script>

<style scoped>
.message-container {
  color: #004085;
  background-color: #cce5ff;
  border: 2px solid #b8daff;
  border-radius: 5px;
  padding: 10px;
  max-width: 400px;
  margin: 30px auto;
  text-align: center;
}
</style>
