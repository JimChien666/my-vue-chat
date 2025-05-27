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
      isTyping: false,
    };
  },
  methods: {
    toggleChat() {
      this.showChat = !this.showChat;
    },
    async handleSend(userInput) {
      this.messages.push({ role: 'user', content: userInput });
      this.isTyping = true;

      try {
        const response = await fetch('http://localhost:8080/api/ask', {
          method: 'POST', // 或改成 'POST'，視後端而定
          headers: {
             'Content-Type': 'application/json',
          },
          body: JSON.stringify({ message: userInput }), // 如果是POST
        });

        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }

        const data = await response.json();
        this.messages.push({ role: 'assistant', content: data.msg });
      } catch (error) {
        console.error('Fetch error:', error);
        this.messages.push({ role: 'assistant', content: '伺服器錯誤，請稍後再試' });
      } finally {
        this.isTyping = false;
      }
    }
  }
};
</script>