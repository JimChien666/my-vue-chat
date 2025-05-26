<template>
  <div class="fixed bottom-4 right-4 z-50">
    <!-- 展開視窗 -->
    <div v-if="isOpen" class="w-80 h-[500px] bg-white rounded-xl shadow-lg flex flex-col border">
      <div class="bg-blue-600 text-white p-3 rounded-t-xl flex justify-between items-center">
        <span class="font-bold">AI 聊天室</span>
        <button @click="isOpen = false" class="text-white">✕</button>
      </div>

      <div class="flex-1 overflow-y-auto p-3 space-y-2">
        <div
          v-for="(msg, idx) in messages"
          :key="idx"
          :class="[
            'p-2 rounded-lg max-w-[75%]',
            msg.sender === 'user'
              ? 'bg-blue-100 self-end text-right ml-auto'
              : 'bg-gray-100 self-start',
          ]"
        >
          {{ msg.text }}
        </div>
        <div v-if="loading" class="italic text-gray-500">AI 正在輸入中...</div>
        <div ref="bottomRef" />
      </div>

      <div class="p-3 border-t flex">
        <textarea
          rows="2"
          v-model="input"
          @keydown.enter.exact.prevent="sendMessage"
          @keydown.enter.shift.stop
          class="flex-1 border rounded p-2 resize-none text-sm"
          placeholder="輸入訊息..."
        ></textarea>
        <button
          @click="sendMessage"
          class="ml-2 px-3 py-1 bg-blue-500 text-white rounded hover:bg-blue-600"
        >
          發送
        </button>
      </div>
    </div>

    <!-- 圓形開啟按鈕 -->
    <button
      v-else
      @click="isOpen = true"
      class="bg-blue-600 text-white p-4 rounded-full shadow-lg hover:bg-blue-700"
    >
      💬
    </button>
  </div>
</template>

<script setup>
import { ref, watch, nextTick } from 'vue'

const isOpen = ref(false)
const messages = ref([{ sender: 'ai', text: '你好！有什麼我可以幫忙的嗎？' }])
const input = ref('')
const loading = ref(false)
const bottomRef = ref(null)

watch(messages, async () => {
  await nextTick()
  bottomRef.value?.scrollIntoView({ behavior: 'smooth' })
})

const sendMessage = async () => {
  if (!input.value.trim()) return
  messages.value.push({ sender: 'user', text: input.value })
  loading.value = true
  const userInput = input.value
  input.value = ''

  try {
    const res = await fetch('http://localhost:3000/api/chat', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ message: userInput }),
    })
    const data = await res.json()
    messages.value.push({ sender: 'ai', text: data.reply })
  } catch (err) {
    messages.value.push({ sender: 'ai', text: '抱歉，發生錯誤了。' })
  } finally {
    loading.value = false
  }
}
</script>