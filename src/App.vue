<!-- parti e html  -->
<template>
  <main
    class="container d-flex flex-column align-items-center mt-5 pt-2"
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
                <label for="date" class="form-label">Date</label>

                <input
                  type="date"
                  class="form-control"
                  id="date"
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
            </form>
          </div>
        </div>
      </div>
    </div>

    <div
      class="container-fluid d-flex algin-items-center justify-content-between mb-4 p-0"
    >
      <span class="fs-2 fw-bold">TODO</span>
      <button
        class="btn btn-primary"
        data-bs-toggle="modal"
        data-bs-target="#staticBackdrop"
      >
        +
      </button>
    </div>
    <p class="align-self-start fs-4">En cours</p>
    <VueDraggable class="container-fluid" ref="el" v-model="todos">
      <li
        v-for="element in todos.filter((t) => !t.fini)"
        :key="element.id"
        :class="{ fini: element.fini }"
        class="d-flex algin-items-center justify-content-between"
        style="position: relative"
      >
        <button
          style="
            width: 100%;
            height: 100%;
            position: absolute;
            top: 0;
            left: 0;
            opacity: 0;
            z-index: 1;
          "
          data-bs-toggle="modal"
          :data-bs-target="'#' + element.id + 'modal'"
        ></button>

        <div style="z-index: 999" class="d-flex algin-items-center gap-3">
          <input
            type="checkbox"
            :checked="element.fini"
            @change="valide(element, $event)"
          />
          <span>{{ element.titre }}</span>
        </div>
        <span
          v-if="element.date"
          style="color: rgb(122, 99, 99); font-size: 12px"
        >
          <i class="bi bi-calendar2-check"></i> {{ element.date }}
        </span>
        <!-- <button @click="deleteTodo(element)" style="z-index: 999">
          <i class="bi bi-trash3-fill"></i>
        </button> -->
      </li>
    </VueDraggable>

    <p class="align-self-start fs-4">Finis</p>

    <VueDraggable class="list-group container-fluid" ref="el" v-model="todos">
      <div
        v-for="element in todos.filter((t) => t.fini)"
        :key="element.id"
        :class="{ fini: element.fini }"
        class="list-group-item d-flex algin-items-center justify-content-between"
        style="position: relative"
      >
        <button
          style="
            width: 100%;
            height: 100%;
            position: absolute;
            top: 0;
            left: 0;
            opacity: 0;
            z-index: 1;
          "
          data-bs-toggle="modal"
          :data-bs-target="'#' + element.id + 'modal'"
        ></button>

        <div style="z-index: 999">
          <input
            type="checkbox"
            :checked="element.fini"
            @change="valide(element, $event)"
          />
          {{ element.titre }}
        </div>
        <button
          @click="deleteTodo(element)"
          style="z-index: 999; background-color: white; color: crimson"
          class="border border-0"
        >
          <i class="bi bi-trash2"></i>
        </button>
      </div>
    </VueDraggable>

    <!-- 
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
    </ol> -->

    <div
      v-for="d in todos"
      class="modal fade"
      :id="d.id + 'modal'"
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
              modifier to do
            </h1>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <form @submit.prevent="modifierTodo(d, $event)">
              <div class="mb-3">
                <label for="Titre" class="form-label">Titre</label>
                <input
                  type="text"
                  class="form-control"
                  id="Titre"
                  :value="d.titre"
                />
                <label for="Details" class="form-label">Description</label>
                <textarea
                  class="form-control"
                  id="Details"
                  :value="d.details"
                ></textarea>
              </div>
              <div class="mb-3">
                <label for="date" class="form-label">Date</label>

                <input
                  type="date"
                  class="form-control"
                  id="date"
                  :value="d.date"
                />
              </div>
              <button class="btn btn-primary" data-bs-dismiss="modal">
                modifier
              </button>

              <button
                class="ms-3"
                @click="deleteTodo(d)"
                style="z-index: 999"
                data-bs-dismiss="modal"
              >
                <i class="bi bi-trash2"></i>
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<!-- partie java script -->
<script setup>
import { ref, onMounted } from "vue";
import { VueDraggable } from "vue-draggable-plus";
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

function modifierTodo(d, e) {
  console.log(e.target.Titre.value);
  console.log(e.target.Details.value);
  console.log(e.target.date.value);

  todos.value = todos.value.map((todo) => {
    if (todo === d) {
      return {
        ...todo,
        titre: e.target.Titre.value,
        details: e.target.Details.value,
        date: e.target.date.value,
      };
    }
    return todo;
  });
  localStorage.setItem("todos", JSON.stringify(todos.value));
} // parcourir tous les element du tableau
</script>

<style>
body {
  background-color: rgb(231, 222, 239);
}

.fini {
  text-decoration: line-through;
  color: rgb(14, 14, 15);
}
input[type="checkbox"] {
  appearance: none;
  -moz-appearance: none;
  -webkit-appearance: none;
  width: 20px;
  height: 20px;
  border: 1px solid green;
  border-radius: 50%;
  margin: auto !important;
}
input[type="checkbox"]:checked {
  background-color: rgb(93, 155, 101);
  position: relative;
}
input[type="checkbox"]:checked::before {
  content: "\2713";
  color: white;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
li {
  background-color: white;
  margin-bottom: 15px;
  border-radius: 5px;
  padding: 15px;
  -webkit-box-shadow: 0px 5px 15px -5px #000000;
  box-shadow: 0px 5px 15px -5px #000000;
}
</style>
