<template>
    <div>
        Temps : {{ time }}
    </div>
</template>

<script setup>
import {onMounted, onUnmounted, ref } from 'vue'

const time = ref(0)

let timer
onMounted(()=>{
    setInterval(()=>{
        console.log('timer')
        time.value ++
    },1000)
})

//On utilise Unmounted pour effacer afin de ne pas avoir de fuite et d'effet de bord
onUnmounted(()=>{
    clearInterval(timer)
})
</script>