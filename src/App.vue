<template>
  <div>
    <Layout>
      <template v-slot:header>En tete</template> <!--v-slot:header peut etre remplacer par <template #header>-->
      <template v-slot:aside>Aside</template>
      <template v-slot:main>Main</template>
      <template v-slot:footer>Footer</template>
    </Layout>
    <form @submit.prevent="ajouterTache">
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
import {computed, ref} from 'vue';
import Checkbox from './Checkbox.vue';
import Button from './Button.vue';
import Layout from './Layout.vue';

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
Decomposition de composnat:
Il faut importer le composant 
Ensuite le mettre dans le html 
On peut definir de props => parametre d'entré de fonction qui permet d'envoyé de l'information au compposant
On peut emettre des events grace au emits
Pour un systeme qui doit comporter les deux elements ce sera le defineModel
Si le composant a des enfants on utilise le systeme de slot
-->