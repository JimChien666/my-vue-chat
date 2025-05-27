<template>
  <div class="chat-header">
    AI 聊天
    <button @click="$emit('close')">-</button>
  </div>
</template>

<style scoped>
.chat-header {
  background: #1877f2;
  color: white;
  padding: 10px;
  display: flex;
  justify-content: space-between;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}
</style>
