<!-- parti e html  -->
<template>
  <form @submit.prevent="ajouterTodo()">
    <input
      v-model="nexTodo"
      type="text"
      @input="test"
      placeholder="new todo..."
    />
    <button>Ajouter</button>
    <ol>
      <p v-if="todos.length === 0">pas de todo !</p>
      <li v-for="element in todos" :key="element.id">
        <input type="checkbox" @v-model="t.fini" />
        {{ element.details }}
        <button @click="deleteTodo(element)">Delete</button>
      </li>
    </ol>
  </form>
</template>

<!-- partie java script -->
<script setup>
import { ref, onMounted } from "vue";
const todos = ref([]);
//tablaeu qui contient tous les tache ecrit par user

const nexTodo = ref("");
onMounted(() => {
  const todosSave = localStorage.getItem("todos");
  if (todosSave) {
    todos.value = JSON.parse(todosSave);
  } else {
    todos.value = [];
  }
});

function ajouterTodo() {
  if (nexTodo.value === "") return; // ne pas ajouter si vide
  todos.value.push({
    id: new Date().getTime(),
    details: nexTodo.value,
    fini: false,
  }); // ajouter en ordre

  nexTodo.value = ""; //// supprimer la val apres lAjout
  localStorage.setItem("todos", JSON.stringify(todos.value));
}

function deleteTodo(todo) {
  todos.value = todos.value.filter((element) => element !== todo); // a l'aide du filtrage on supprime
  localStorage.setItem("todos", JSON.stringify(todos.value));
}
</script>
