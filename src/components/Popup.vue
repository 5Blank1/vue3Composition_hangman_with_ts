<template>
  <div v-show="isVisible" class="popup-container">
    <div class="popup">
      <h2 v-if="gameStatus === 'win'">Поздравляю, вы победили! 😃</h2>
      <template v-else>
      <h2>Вы проиграли. 😕</h2>
      <h3>...имя: {{word}}</h3>
      </template>
      <button @click="emit('restart')">Сыграть еще раз</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import {defineProps, defineExpose, defineEmits, ref } from "vue";
import {GameStatus} from "@/types/GameStatus";

interface Props {
  word: string
}
defineProps<Props>()

const gameStatus = ref<GameStatus | null>(null)
const isVisible = ref(false)

const emit = defineEmits<{
  (e: 'restart'): void
}>()

const handleOpen = (status: GameStatus) => {
  gameStatus.value = status
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
