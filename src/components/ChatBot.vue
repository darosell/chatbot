
<template>
  <div class="chatbox-container">
    <div class="container">
      <h1>Chat Bot</h1>
      <div id="chatId" class="messageBox mt-8">
        <template v-for="(message, index) in messages" :key="index">
          <div :class="message.from == 'user' ? 'messageFromUser' : 'messageFromChat'">
            <div :class="message.from == 'user' ? 'userMessageWrapper' : 'chatMessageWrapper'">
              <div :class="message.from == 'user' ? 'userMessageContent' : 'chatMessageContent'">{{ message.data }}
              </div>
              <!-- <div :class="inline-but" v-for="(option, j) in message.options" :key="j">
          <button @click="sendMessage(j,option)">{{ option }}</button>
        </div> -->
            </div>
          </div>

         </template> 
            <div id="opcionesChatContenedor">
              <div  id="opcionesChat" v-for="(option, j) in buttons" :key="j">
                <button class="btn secondary" @click="sendMessage(j, option)">{{ option }}</button>
              </div>
            </div>
            
            
        
      </div>
      <!-- <div class="inputContainer">
        <div v-for="(option, j) in buttons" :key="j">
          <button class="btn secondary" @click="sendMessage(j, option)">{{ option }}</button>
        </div>
      </div> -->
      <!-- <div class="inputContainer">
  <input
    v-model="currentMessage"
    type="text"
    class="messageInput"
    placeholder="Respuesta..."
  />
  <button
    @click="sendMessage(currentMessage)"
    class="askButton"
  >
    Enviar
  </button>
</div> -->
    </div>

  </div>
</template>




<script>
import axios from 'axios';
// import {ref} from "vue";

export default {
  name: 'ChatBot',
  data() {
    return {
      currentMessage: '',
      messages: [],
      buttons: []
    }
  },
  methods: {
    async sendMessage(optionNumber, optionDescription) {

      this.messages.push({
        from: 'user',
        data: optionDescription,
      });
      this.messages.push({
        from: 'chat',
        data: '...',
        options: ''
      });
      await axios
        .post('https://strengthened-honey-bestseller.glitch.me/sendSelection', {
          respuesta: optionNumber,
        })
        .then((response) => {
          this.messages.pop();
          this.messages.push({
            from: 'chat',
            data: response.data.description,
            options: response.data.options
          });
          this.buttons = response.data.options;
          this.messages[0].options = [];

          var objDiv = document.getElementById("chatId");
          console.log("1: " + objDiv.scrollTop + " 2: " + objDiv.scrollHeight)

          objDiv.scrollTop = objDiv.scrollHeight;
          // $("#chatId").animate({ scrollTop: $('#chatId').prop("scrollHeight")}, 1000);
        });

    },
  },

  mounted() {
    axios.get('https://strengthened-honey-bestseller.glitch.me/getData', {
      // respuesta: message,
    })
      .then((response) => {
        this.messages.push({
          from: 'chat',
          data: response.data.description,
          options: response.data.options
        });
        this.buttons = response.data.options;
      })
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap');

.chatbox-container {
  /* position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1000; */
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100vh;
}

.container {
  width: 100%;
  height: 100vh;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  font-family: 'Roboto', sans-serif;
}

h1 {
  font-size: 24px;
  font-weight: 500;
  text-align: center;
  color: #222;
  padding: 16px;
  margin: 0;
  background-color: #f7f7f7;
  border-bottom: 1px solid #e7e7e7;
}

.btn {
  border: none;
  color: black;
  padding: 14px 24px;
  cursor: pointer;
  border-radius: 5px;
  /* margin:10px 10px 30px 10px; */
}

.primary {
  background-color: white;
  border: 2px solid #007bff;
}

.primary:hover {
  background: #0b7dda;
  color: white;
}

.secondary {
  background-color: white;
  border: 2px solid #e7e7e7;          
}

.secondary:hover {
  background: #ddd;
  color: white;
}

.inline-but {
  display: inline-block;
}

.messageBox {
  padding: 16px;
  flex-grow: 1;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.messageFromUser,
.messageFromChat {
  /* display: flex; */
  display: contents;
  text-align: center;
}

.messageBox {
  max-height: 80vh;
  overflow-y: auto;
  padding: 0 16px;
  border-top: 1px solid #f0f0f0;
  border-bottom: 1px solid #f0f0f0;
  flex-grow: 1;
}

.messageFromUser,
.messageFromChat {
  /* display: flex; */
  padding-right: 0%;
  margin-bottom: 8px;
}

.userMessageWrapper,
.chatMessageWrapper {
  display: flex;
  flex-direction: column;
}

.userMessageWrapper {
  align-self: flex-end;
}

.chatMessageWrapper {
  align-self: flex-start;
}

.userMessageContent,
.chatMessageContent {
  max-width: 100%;
  padding: 8px 12px;
  border-radius: 18px;
  margin-bottom: 2px;
  font-size: 14px;
  line-height: 1.4;
}

.userMessageContent {
  background-color: #1877F2;
  color: white;
  border-top-right-radius: 0;
}

.chatMessageContent {
  background-color: #EDEDED;
  color: #222;
  border-top-left-radius: 0;
}

.userMessageTimestamp,
.chatMessageTimestamp {
  font-size: 10px;
  color: #999;
  margin-top: 2px;
}

.userMessageTimestamp {
  align-self: flex-end;
}

.chatMessageTimestamp {
  align-self: flex-start;
}

.inputContainer {
  flex-direction: row;
  display: flex;
  align-items: center;
  padding: 10px;
  background-color: #f0f0f0;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
  /* min-height: 90px; */
}


.messageInput {
  flex-grow: 1;
  border: none;
  outline: none;
  padding: 12px;
  font-size: 16px;
  background-color: white;
  border-radius: 24px;
  margin-right: 8px;
}

.askButton {
  background-color: #1877F2;
  color: white;
  font-size: 16px;
  padding: 8px 16px;
  border: none;
  outline: none;
  cursor: pointer;
  border-radius: 24px;
  transition: background-color 0.3s ease-in-out;
}

.askButton:hover {
  background-color: #145CB3;
}

@media (max-width: 480px) {
  .container {
    width: 100%;
    max-width: none;
    border-radius: 0;
  }
}

.chatbox-container {
  position: fixed;
  /* bottom: 24px;
  right: 24px; */
  z-index: 1000;
}


.messageBox {
  padding-top: 16px;
  padding-left: 25%;
  padding-right: 25%;
  flex-grow: 1;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

/* .messageFromUser
.messageFromChat {
  display: flex;
} */

#opcionesChatContenedor{
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: left;
  /* background-color: #EDEDED; */
  /* color: #222; */
  border-radius: 18px;
  border-top-left-radius: 0;
  padding-block: 8px;
  padding-inline: 10px;
  margin-bottom: 2px;
  line-height: 1.4;
}

/* #opcionesChat{
  padding: 8px, 12px;
  margin-bottom: 2px;
  line-height: 1.4;
} */
</style>