<template>
  <div class="chat-footer">
    <input v-model="input" @keydown.enter="send" placeholder="Aa" />
    <button
      class="send-button"
      :class="{ active: isActive }"
      @mousedown="isActive = true"
      @mouseup="onClick"
      @mouseleave="isActive = false"
      :disabled="!input.trim()"
      aria-label="送出"
    >
      <svg
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <polygon
          :class="{ filled: isActive }"
          stroke="#1877f2"
          stroke-width="2"
          points="6,4 20,12 6,20"
        />
      </svg>
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      input: '',
      isActive: false,
    };
  },
  methods: {
    send() {
      const value = this.input.trim();
      if (value) {
        this.$emit('send', value);
        this.input = '';
      }
      this.isActive = false;
    },
    onClick() {
      this.send();
      this.isActive = false;
    },
  },
};
</script>

<style scoped>
.chat-footer {
  display: flex;
  border-top: 1px solid #eee;
  padding: 5px;
  align-items: center;
}

.chat-footer input {
  flex: 1;
  padding: 8px 12px;
  font-size: 14px;
  border: 1px solid #ccc; /* 較淺的邊框色 */
  border-radius: 12px; /* 圓角 */
  outline: none;
  transition: border-color 0.2s ease;
}

/* 聚焦時邊框色變淡藍，不要粉紅色 */
.chat-footer input:focus {
  border-color: #1877f2;
  box-shadow: 0 0 5px rgba(24, 119, 242, 0.4);
}

/* 取消點擊時可能出現的粉紅色外框(Chrome autofill focus) */
.chat-footer input::-webkit-autofill,
.chat-footer input:-webkit-autofill,
.chat-footer input:focus {
  box-shadow: none !important;
  -webkit-box-shadow: none !important;
  outline: none !important;
}
</style>