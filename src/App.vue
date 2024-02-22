<!-- parti e html  -->
<template>
  <form>
    <input type="text" />
    <button>Ajouter</button>
    <ul>
      <li>Faire du sport</li>
      <li>apprendre React</li>
      <li>Lire un livre</li>
    </ul>
  </form>
</template>

<!-- partie java script -->
<script>
import { ref } from "vue";
const todos = ref([]);
</script>
