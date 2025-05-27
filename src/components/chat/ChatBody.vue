<template>
  <div class="chat-body">
    <div class="chat-messages">
      <div
        v-for="(msg, index) in messages"
        :key="index"
        class="chat-message"
        :class="msg.role"
      >
        <div class="message-bubble" :class="msg.role">
          {{ msg.content }}
        </div>
      </div>

      <div v-if="isTyping" class="chat-message assistant">
        <div class="message-bubble assistant typing-bubble">
          <span class="dot"></span>
          <span class="dot"></span>
          <span class="dot"></span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    messages: Array,
    isTyping: Boolean,
  },
  watch: {
    messages() {
      this.$nextTick(() => {
        const container = this.$el;
        container.scrollTop = container.scrollHeight;
      });
    },
  },
};
</script>

<style scoped>
.chat-body {
  padding: 10px;
  font-size: 14px;
  box-sizing: border-box;
  overflow-y: auto;
  flex: 1;
}

.chat-messages {
  display: flex;
  flex-direction: column;
}

.chat-message {
  margin-bottom: 8px;
}

.chat-message.user {
  display: flex;
  justify-content: flex-end;
}

.chat-message.assistant {
  display: flex;
  justify-content: flex-start;
}

.message-bubble {
  padding: 8px 12px;
  border-radius: 16px;
  max-width: 70%;
  word-break: break-word;
}

.message-bubble.user {
  background-color: #1877f2;
  color: white;
  border-bottom-right-radius: 2px;
}

.message-bubble.assistant {
  background-color: #f1f0f0;
  color: black;
  border-bottom-left-radius: 2px;
}

.typing-bubble {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  width: 40px;
  padding: 8px 10px;
}

.dot {
  width: 6px;
  height: 6px;
  margin: 0 3px;
  background-color: #999;
  border-radius: 50%;
  opacity: 0.3;
  animation: blink 1.4s infinite both;
}

.dot:nth-child(1) {
  animation-delay: 0s;
}
.dot:nth-child(2) {
  animation-delay: 0.2s;
}
.dot:nth-child(3) {
  animation-delay: 0.4s;
}

@keyframes blink {
  0%, 80%, 100% {
    opacity: 0.3;
  }
  40% {
    opacity: 1;
  }
}
</style>