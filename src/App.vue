<template>
  <Header/>
  <div class="game-container">
    <Figure :false-letters-count="falseLetters.length"/>
    <WrongLetters :falseLetters="falseLetters"/>
    <TrueWord :word="word" :correctLetters="correctLetters"/>
  </div>
  <Popup ref="popup" :word="word" @restart="restart"/>
  <Notification ref="notification"/>
</template>

<script setup lang="ts">
import Header from './components/Header.vue'
import Figure from './components/Figure.vue'
import WrongLetters from "@/components/WrongLetters.vue";
import TrueWord from "@/components/TrueWord.vue";
import Popup from "@/components/Popup.vue";
import Notification from "@/components/Notification.vue";
import {computed, ref, watch} from "vue";
import axios from "axios";

const word = ref('')
const letters = ref<string[]>([])
const correctLetters = computed(() => letters.value.filter(x => word.value.includes(x)))
const falseLetters = computed(() => letters.value.filter(x => !word.value.includes(x)))
const isStatusLose = computed(() => falseLetters.value.length === 6)
const isStatusWin = computed(() => [...word.value].every(x => correctLetters.value.includes(x)))
const notification = ref<InstanceType<typeof Notification> | null>(null)
const popup = ref<InstanceType<typeof Popup> | null>(null)

interface ApiResponse {
  FirstName: string;
}


const fetchName = async () => {
  try {
    const response = await axios.get<ApiResponse>('https://api.randomdatatools.ru/?unescaped=false&params=FirstName&_=' + new Date().getTime())
    console.log(response.data)
    const data = response.data
    word.value = data.FirstName ? data.FirstName.toLowerCase() : ''
  } catch (e) {
    console.log(e)
    word.value = ''
  }
}

fetchName()

const restart = async () => {
  await fetchName()
  letters.value = []
  popup.value?.handleClose()
}

watch(falseLetters, () => {
  if(isStatusLose.value) {
    popup.value?.handleOpen('lose')
  }
})

watch(correctLetters, () => {
  if(isStatusWin.value){
    popup.value?.handleOpen('win')
  }
})

window.addEventListener('keydown', ({key}) => {
  if(isStatusLose.value || isStatusWin.value) {
    return
  }
  if(letters.value.includes(key)) {
    notification.value?.handleOpen()
    setTimeout(() => {
      notification.value.handleClose()
    }, 2000)
    return
  }

  if(/[а-яА-ЯёЁ]/.test(key)) {
    letters.value.push(key.toLowerCase())
  }
})

</script>
