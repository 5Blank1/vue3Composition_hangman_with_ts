<template>
  <div class="word">
    <span class="letter" v-for="(letter, index) in word" :key="index">{{correctLetters.includes(letter) ? letter : ''}}</span>
  </div>
</template>

<script setup lang="ts">
import {defineProps } from "vue";
interface Props {
  word: string,
  correctLetters: string[]
}

defineProps<Props>()

</script>

<style scoped>

</style>
