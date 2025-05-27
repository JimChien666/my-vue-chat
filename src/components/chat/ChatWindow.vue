<template>
  <div class="chat-window">
    <ChatHeader @close="$emit('close')" />
    <ChatBody :messages="messages" :isTyping="isTyping" />
    <ChatFooter @send="$emit('send', $event)" />
  </div>
</template>

<script>
import ChatHeader from './ChatHeader.vue';
import ChatBody from './ChatBody.vue';
import ChatFooter from './ChatFooter.vue';

export default {
  components: { ChatHeader, ChatBody, ChatFooter },
  props: {
    messages: Array,
    isTyping: Boolean  // 這裡也要接收 isTyping prop
  }
};
</script>

<style scoped>
.chat-window {
  position: fixed;
  bottom: 90px;
  right: 20px;
  width: 300px;
  height: 400px; /* ← 確保總高度夠 */
  max-height: 400px;
  background: white;
  border: 1px solid #ccc;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  box-shadow: 0 4px 12px rgba(0,0,0,0.2);
}
</style>