<template>
    <label >
        <!--@change="onChange" dans l'input-->
        <input type="checkbox"  v-model="model"> {{ label }}
    </label>
</template>

<script setup>
const model = defineModel()
// defineProps permet de diffuser l'info au parent
const props = defineProps({
    label: String,
})
//defineEmits permet de diffuser un evenement au parent 
const emits = defineEmits([
    'check','uncheck'
])
const onChange =(e)=>{
    if(e.currentTarget.checked){
        emits('check', e.currentTarget)
    }else{
        emits('uncheck', e.currentTarget)
    }
}

</script>