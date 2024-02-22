<!-- parti e html  -->
<template>
  <form @submit.prevent="ajouterTodo()">
    <input v-model="nexTodo" type="text" @input="test" />
    <button>Ajouter</button>
    <ol>
      <li v-for="element in todos" :key="element.id">
        {{ element.details }}
        <button @click="deleteTodo(element)">Delete</button>
      </li>
    </ol>
  </form>
</template>

<!-- partie java script -->
<script setup>
import { ref } from "vue";
const todos = ref([]);
//tablaeu qui contient tous les tache ecrit par user

const nexTodo = ref("");

function ajouterTodo() {
  if (nexTodo.value === "") return; // ne pas ajouter si vide
  todos.value.push({ id: new Date().getTime(), details: nexTodo.value }); // ajouter en ordre
  nexTodo.value = ""; //// supprimer la val apres lAjout
}

function deleteTodo(todo) {
  todos.value = todos.value.filter((element) => element !== todo);
}
</script>
