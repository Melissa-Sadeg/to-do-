<!-- parti e html  -->
<template>
  <main
    class="container border d-flex flex-column align-items-center mt-5 pt-2"
    style="max-width: 705px; z-index: 999"
  >
    <!-- <form @submit.prevent="ajouterTodo()">
      <input
        v-model="title"
        type="text"
        @input="test"
        placeholder="new todo..."
      />
      <button class="btn btn-outline-success">Ajouter</button>
    </form> -->
    <!-- Modal -->
    <div
      class="modal fade"
      id="staticBackdrop"
      data-bs-backdrop="static"
      data-bs-keyboard="false"
      tabindex="-1"
      aria-labelledby="staticBackdropLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="staticBackdropLabel">
              Créer un ToDo
            </h1>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <form @submit.prevent="ajouterTodo()">
              <div class="mb-3">
                <label for="Titre" class="form-label">Titre</label>
                <input
                  type="text"
                  class="form-control"
                  id="Titre"
                  v-model="title"
                />
                <label for="Details" class="form-label">Description</label>
                <textarea
                  class="form-control"
                  id="Details"
                  v-model="Description"
                ></textarea>
              </div>
              <div class="mb-3">
                <label for="date-limite" class="form-label">Date</label>

                <input
                  type="date"
                  class="form-control"
                  id="date-limite"
                  v-model="date"
                />
              </div>
              <button
                class="btn btn-primary"
                data-bs-dismiss="modal"
                :disabled="!title"
              >
                Save
              </button>

              <div class="mb-3 form-check"></div>
            </form>
          </div>
        </div>
      </div>
    </div>

    <div
      class="container-fluid d-flex algin-items-center justify-content-between mb-4"
    >
      <span class="fs-2 fw-bold">TODO</span>
      <button
        class="btn btn-primary"
        data-bs-toggle="modal"
        data-bs-target="#staticBackdrop"
      >
        Ajouter une tache
      </button>
    </div>

    <ol class="list-group container-fluid">
      <p v-if="todos.length === 0">pas de todo !</p>
      <li
        d-flex
        flex-column
        align-items-center
        class="list-group-item d-flex algin-items-center justify-content-between"
        v-for="element in todos"
        :key="element.id"
        :class="{ fini: element.fini }"
      >
        <div>
          <input
            type="checkbox"
            :checked="element.fini"
            @change="valide(element, $event)"
          />
          {{ element.titre }}
        </div>
        <button @click="deleteTodo(element)">
          <i class="bi bi-trash3-fill"></i>
        </button>
      </li>
    </ol>
  </main>
</template>

<!-- partie java script -->
<script setup>
import { ref, onMounted } from "vue";
const title = ref("");
const Description = ref("");
const date = ref("");

const todos = ref([]);
//tablaeu qui contient tous les tache ecrit par user

onMounted(() => {
  const todosSave = localStorage.getItem("todos");
  if (todosSave) {
    todos.value = JSON.parse(todosSave);
  } else {
    todos.value = [];
  }
});

function ajouterTodo() {
  if (title.value === "") return;
  console.log("Title :" + title.value);
  console.log("Description :" + Description.value);
  console.log("Date :" + date.value);

  // ne pas ajouter si vide
  todos.value.push({
    id: new Date().getTime(),
    details: Description.value,
    titre: title.value,
    date: date.value,

    fini: false,
  }); // ajouter en ordre

  title.value = ""; //// supprimer la val apres lAjout
  Description.value = "";
  date.value = "";
  localStorage.setItem("todos", JSON.stringify(todos.value));
}

function deleteTodo(todo) {
  todos.value = todos.value.filter((element) => element !== todo); // a l'aide du filtrage on supprime
  localStorage.setItem("todos", JSON.stringify(todos.value));
}
function valide(element, e) {
  element.fini = e.target.checked;
  localStorage.setItem("todos", JSON.stringify(todos.value));
}
</script>

<style>
.fini {
  text-decoration: line-through;
  color: blue;
  background-color: rgb(244, 236, 225);
}
</style>
