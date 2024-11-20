<template>
  <input type="text" v-model="page.title">
  Temps écoulé: {{  time }}
  <button @click="reset">Remttre a zero</button>
</template>

<script setup>
import { ref, watchEffect } from 'vue'
import { useTimer } from './composable/useTimer';


const {time, reset }= useTimer() //fonction personnalisé

const page = ref({
  title:''
})

watchEffect(()=>{
  document.title = page.value.title
})
</script>


<!--
Les watcher servent a observer les changeement de valeur de maniere reactive et faire de la logique en fonction de ce changement

-->