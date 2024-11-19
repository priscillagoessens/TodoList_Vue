<template>
  <div>
    <Timer/>
    <form @submit.prevent="addTodo">
      <fieldset role="group">
      <input type="text" placeholder="Ajouter une tache" v-model="newTodo">
      <button :disabled="newTodo.length ==0">Ajouter</button>
    </fieldset>
    </form>
    <p v-if="todos.length == 0 ">La liste est vide</p>
    <div v-else>
      <ul>
        <li v-for="todo in sortedTodos" :key="todo.date" :class="{completed: todo.completed}">
        <label>
          <!--Le v-model sert a retransmettre l'element même lorsqu'il est modifier en direct dans l'input-->
          <!--<input type="checkbox" v-model="todo.completed">{{ todo.title }}-->
          <Checkbox :label="todo.title" v-model="todo.completed"/>
        </label>
      </li>
      </ul>
      <label>
        <input type="checkbox" v-model="hideCompleted"> Masquer les taches complétées
      </label>
      <p v-if="remainingTodos >0">Nombres de tache{{ remainingTodos > 1 ?'s' : '' }} restante : {{ remainingTodos }}</p>
      <Checkbox label="Bonjour"/>
    </div>
  </div>
</template>

<script setup>
import {computed, onMounted, ref} from 'vue';
import Checkbox from './Checkbox.vue';
import Button from './Button.vue';
import Layout from './Layout.vue';
import Timer from './Timer.vue';

const newTodo = ref('')
const hideCompleted= ref(false)
const todos = ref([])


onMounted(()=>{
  fetch('https://jsonplaceholder.typicode.com/todos ')
  .then(res => res.json())
  .then(v => todos.value = v.map(todo =>({ ...todo, date: todo.id})))
})

const addTodo=()=>{
  todos.value.push({
    title: newTodo.value,
    completed: false,
    date: Date.now()
  })
  newTodo.value = ''
}

const sortedTodos= computed(()=>{
  const sortedTodos = todos.value.toSorted((a,b)=> a.completed>b.completed ? 1 : -1)
  if(hideCompleted.value == true){
    return sortedTodos.filter(t=>t.completed == false)
  }
  return sortedTodos
})

//affiche le nombre de taches a faire
const remainingTodos = computed(()=>{
  return todos.value.filter(t=> t.completed == false).length
})
</script>

<style>
.completed{
  text-decoration: line-through;
}
</style>

<!--
Appeler la liste de taches depuis un serveur quand le composant est chargé
jsonplaceholder.typicode.com/todos

-->