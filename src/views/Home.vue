<template>
  <div class="mail-box">
    <app-sidebar :messages="messages"></app-sidebar>
    <app-content :messages="messages"></app-content>
  </div>
</template>
<script>
import Sidebar from "../components/Sidebar.vue";
import Content from '../components/Content.vue'
import messages from '../messages'
import randomMessages from '../random-messages'
import { eventBus } from "../main";

  export default {
    data() {
      return {
        messages: messages,
        randomMessages
      }
    },
    created() {
      eventBus.$on('sentMessage', (data) => {
        let temp = [data.message];
        this.messages = temp.concat(this.messages.slice(0))
      });
      eventBus.$on('refreshMessages', ()=> {
        
        let randomIndex = Math.floor(Math.random() * this.randomMessages.length)
        
        let temp =  [this.randomMessages[randomIndex]]
        
        this.messages = temp.concat(this.messages.slice(0))
      }) 
    },
    components: {
      appSidebar: Sidebar,
      appContent: Content
    }
  }
</script>

