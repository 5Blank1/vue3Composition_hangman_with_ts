<template>
  <div class="notification-container" :class="{show: isVisible}">
    <p>Вы уже вводили этот символ</p>
  </div>
</template>

<script setup lang="ts">
import {ref, defineExpose } from "vue";

const isVisible = ref(false)

const handleOpen = () => {
  isVisible.value = true
}

const handleClose = () => {
  isVisible.value = false
}

defineExpose({
  handleOpen,
  handleClose
})
</script>

<style scoped>

</style>
