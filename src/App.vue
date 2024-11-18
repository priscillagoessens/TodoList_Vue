<template>
  <form @submit.prevent="ajouterTache">
    <fieldset role="group">
    <input type="text" placeholder="Ajouter une tache" v-model="newTodo">
    <button :disabled="newTodo.length ==0">Ajouter</button>
  </fieldset>
  </form>
  <p v-if="todos.length == 0 ">La liste est vide</p>
  <div v-else>
    <ul>
      <li v-for="todo in sortedTodos()" :key="todo.date" :class="{completed: todo.completed}">
      <label>
        <input type="checkbox" v-model="todo.completed">{{ todo.title }}
      </label>
    </li>
    </ul>
    <label>
      <input type="checkbox" v-model="hideCompleted"> Masquer les taches complétées
    </label>
  </div>
</template>

<script setup>
import {ref} from 'vue';

const newTodo = ref('')
const todos = ref([{
  title:'Tache de test',
  completed: true,
  date:1
},{
  title:'Tache a faire',
  completed: false,
  date:2
} 
])
const hideCompleted= ref(false)

const ajouterTache=()=>{
  todos.value.push({
    title: newTodo.value,
    completed: false,
    date: Date.now()
  })
  newTodo.value = ''
}

const sortedTodos=()=>{
  const sortedTodos = todos.value.toSorted((a,b)=> a.completed>b.completed ? 1 : -1)
  if(hideCompleted.value == true){
    return sortedTodos.filter(t=>t.completed == false)
  }
  return sortedTodos
}
</script>

<style>
.completed{
  text-decoration: line-through;
}
</style>