<template>
  <div class="game-window" :style="{ height }" @click="focus">
    <div class="viewport" ref="viewportEl">
      <div v-for="(line, i) in renderedLines" :key="i" class="line" v-html="line"></div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch, nextTick, onMounted } from 'vue'

const props = defineProps({
  // Plain text (may contain newlines) or an array of strings. Component will render lines in order.
  content: { type: [String, Array], default: '' },
  // Auto scroll to bottom when content changes
  autoScroll: { type: Boolean, default: true },
  // CSS height (e.g. '300px' or '40vh')
  height: { type: String, default: '320px' },
  // Whether to interpret simple ANSI-like color tokens (e.g. [red]text[/red])
  simpleMarkup: { type: Boolean, default: false }
})

const viewportEl = ref(null)

// Accept either string (split by newlines) or an array of lines
const renderedLines = computed(() => {
  let lines = []
  if (Array.isArray(props.content)) lines = props.content.slice()
  else if (typeof props.content === 'string') lines = props.content.split(/\r?\n/)
  else lines = [String(props.content)]

  if (props.simpleMarkup) {
    return lines.map(line => escapeHtml(line)
        .replace(/\[b\](.*?)\[\/b\]/gi, '<strong>$1</strong>')
        .replace(/\[i\](.*?)\[\/i\]/gi, '<em>$1</em>')
        .replace(/\[red\](.*?)\[\/red\]/gi, '<span class="ansi-red">$1</span>')
        .replace(/\[green\](.*?)\[\/green\]/gi, '<span class="ansi-green">$1</span>')
    )
  }

  return lines.map(escapeHtml)
})

function escapeHtml(s){
  return String(s)
      .replace(/&/g, '&amp;')
      .replace(/</g, '&lt;')
      .replace(/>/g, '&gt;')
}

function scrollToBottom(){
  const el = viewportEl.value
  if (!el) return
  el.scrollTop = el.scrollHeight
}

watch(() => props.content, async () => {
  if (props.autoScroll) await nextTick(scrollToBottom)
})

onMounted(() => {
  if (props.autoScroll) nextTick(scrollToBottom)
})

function focus(){
  // give visual focus if needed; selected text remains selectable
}
</script>

<style scoped>
.game-window{
  box-sizing: border-box;
  background: #0a0a0a;
  border: 2px dotted #666;
  border-radius: 6px;
  padding: 10px;
  width: 100%;
  overflow: hidden;
  font-family: 'JetBrains Mono', 'Courier New', monospace;
  color: #e5e5e5;
}

.viewport{
  height: 100%;
  overflow: auto;
  padding-right: 6px;
}

.line{
  white-space: pre-wrap;
  line-height: 1.35;
  font-size: 13px;
  margin-bottom: 6px;
}

.ansi-red{ color: #ff6b6b }
.ansi-green{ color: #7ce38b }

.game-window:focus-within{
  border-color: #aaa;
}
</style>