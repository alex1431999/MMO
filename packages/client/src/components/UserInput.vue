<script setup>
import { ref, onMounted } from 'vue'

const emit = defineEmits(['send'])
const message = ref('')
const inputEl = ref(null)

function onSend() {
  if (!message.value.trim()) return
  emit('send', message.value.trim())
  message.value = ''
}

onMounted(() => {
  inputEl.value?.focus()
})
</script>

<template>
  <div class="user-input-box">
    <input
        ref="inputEl"
        v-model="message"
        @keydown.enter.prevent="onSend"
        placeholder="Type a message..."
        class="user-input-input"
        type="text"
    />
  </div>
</template>

<style scoped>
.user-input-box {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #0a0a0a;
  border: 2px dotted #666;
  border-radius: 4px;
  padding: 10px;
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  box-sizing: border-box;
}

.user-input-input {
  flex: 1;
  background: transparent;
  border: none;
  outline: none;
  color: #e5e5e5;
  font-family: 'JetBrains Mono', 'Courier New', monospace;
  font-size: 14px;
  caret-color: #ccc;
}

.user-input-input::placeholder {
  color: #555;
}

.user-input-box:focus-within {
  border-color: #aaa;
}
</style>