<template>
  <div>
    <div v-for="(msg, index) in messages" :key="index">
      <p><span class="font-weight-bold">{{ msg.user }}: </span>{{ msg.message }}</p>
    </div>
    <form @submit.prevent="sendMessage">
      <div class="gorm-group">
        <label for="user">User:</label>
          <input type="text" v-model="user" class="form-control">
      </div>
      <div class="gorm-group pb-3">
        <label for="message">Message:</label>
          <input type="text" v-model="message" class="form-control">
      </div>
        <button type="submit" class="btn btn-success">Send</button>
    </form>
  </div>
</template>

<script>
import io from 'socket.io-client'
export default {
  data(){
    return{
      user:'',
      message:'',
      messages:[],
      socket: io('localhost:3001')
    }
  },
  methods:{
    sendMessage(e) {
      e.preventDefault();
      this.socket.emit('SEND_MESSAGE', {
      user: this.user,
      message: this.message
    });
    this.message = ''
    }
  },
  mounted(){
    this.socket.on('MESSAGE', (data) => {
      this.messages = [...this.messages, data];            
    });
  },
  name:'SocketTest'
}
</script>

<style  scoped>

</style>