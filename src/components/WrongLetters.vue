<template>
  <div v-if="falseLetters.length > 0" class="wrong-letters-container">
    <div class="wrong-letters">
      <p>Ошибки</p>
      <span v-for="(letter, index) in falseLetters" :key="index">{{index === falseLetters.length - 1 ? letter : `${letter}, `}}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import {defineProps } from "vue";
interface Props {
  falseLetters: string[]
}

defineProps<Props>()
</script>

<style scoped>

</style>
