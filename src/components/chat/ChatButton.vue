<template>
  <button class="chat-button" @click="$emit('toggle')">
    <svg viewBox="0 0 24 24" fill="white" width="24" height="24">
      <path d="M4 4h16v12H5.17L4 17.17V4zm0-2a2 2 0 0 0-2 2v18l4-4h14a2 2 0 0 0 2-2V4a2 2 0 0 0-2-2H4z"/>
    </svg>
  </button>
</template>

<style scoped>
.chat-button {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background-color: #1877f2;
  border: none;
  border-radius: 50%;
  width: 60px;
  height: 60px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>