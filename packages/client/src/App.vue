<script setup lang="ts">
import UserInput from "@/components/UserInput.vue";
import GameWindow from "@/components/GameWindow.vue";
import {ref} from "vue";

const messages = ref<string[]>([]);
</script>

<template>
  <GameWindow :content="messages" />
  <UserInput @send="message => messages.push(message)" />
</template>
