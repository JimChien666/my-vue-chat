<template>
  <div>
    <ChatButton @toggle="toggleChat" />
    <ChatWindow
      v-if="showChat"
      :messages="messages"
      :isTyping="isTyping"
      @send="handleSend"
      @close="toggleChat"
    />
  </div>
</template>

<script>
import ChatButton from './ChatButton.vue';
import ChatWindow from './ChatWindow.vue';

export default {
  components: { ChatButton, ChatWindow },
  data() {
    return {
      showChat: false,
      messages: [],
      isTyping: false,  // 新增 isTyping 狀態
    };
  },
  methods: {
    toggleChat() {
      this.showChat = !this.showChat;
    },
    async handleSend(userInput) {
      this.messages.push({ role: 'user', content: userInput });
      this.isTyping = true;            // 開始打字動畫

      const aiReply = await new Promise(resolve => {
        setTimeout(() => {
          resolve(`你說的是：「${userInput}」嗎？`);
        }, 1000);  // 模擬 AI 回應時間
      });

      this.isTyping = false;           // 結束打字動畫
      this.messages.push({ role: 'assistant', content: aiReply });
    }
  }
};
</script>