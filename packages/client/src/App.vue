<script setup lang="ts">
import UserInput from "@/components/UserInput.vue";
import GameWindow from "@/components/GameWindow.vue";
</script>

<template>
  <GameWindow :content="['hello', 'there']" />
  <UserInput />
</template>
