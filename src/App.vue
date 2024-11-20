<template>
  <input type="text" v-model="page.title">
</template>

<script setup>
import { ref, watch } from 'vue'

const page = ref({
  title:''
})

watch(()=>page.value.title,(newValue, oldValue)=>{ //watch(()=>page.value.title, utilisation d'un getter : fonction qui renvoie la valeur a observer 
  document.title = newValue
}) // {immediate:true} permet d'avoir le resultat au chargement initial de la page

</script>

<style>

</style>

<!--
Les watcher servent a observer les changeement de valeur de maniere reactive et faire de la logique en fonction de ce changement

-->