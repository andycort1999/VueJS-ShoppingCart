<script setup>
import { ref } from 'vue';
const header = ref('App Lista de compras');
const items = ref([
  {id: 1, label: '10 bolillos', purchased: true, highPriority: false},
  {id: 2, label: '1 lata de frijoles', purchased: false, highPriority: true},
  {id: 3, label: '2 lata de atún', purchased: true, highPriority: true}
]);
// 
const togglePurchased = (item) =>{
  //Invertir la propiedad "purchased"
  item.purchased = !item.purchased;
}
// agreganos metodo para guardar nuevo articulo en la lista 
const saveitem = () => {
  items.value.push({id: items.value.length + 1, label: newItem.value})
  // limpiando el contenido de newiten
  newItem.value="";
}

// Enlazado con la caja de texto
const newItem = ref(''); 
const newItemHighPriority = ref(false);
const editing = ref(false);
const doEdit = (edit) => {
  // Alterar la variable "editing"
  editing.value = edit;
  // Limpiar el input de texto
  newItem.value = "";
}
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button v-if="!editing" @click="doEdit(true)" class="btn btn-primary">
      Ageragar Articulo
    </button>
    <button v-else @click="doEdit(false)" class="btn">
      Cancelar
    </button>
  </div>
  <!-- <a v-bind:href="newItem">
    <i class="material-icons shoppind-cart-icon">link</i>
  </a> -->
  <form v-if="editing" v-on:submit.prevent="saveitem" class="add-item form">
    <!-- Input de Nuevo Articulo -->
    <input v-model.trim="newItem" type="text" placeholder="Ingresar nuevo articulo">
    <!-- Check Boxes -->
    <label>
      <input v-model="newItemHighPriority" 
      type="checkbox">
      Alta Prioridad
    </label>
    {{ newItemHighPriority ? "🔥" : "🧊" }}
    <!-- Boton de UI -->
    <button :disabled="newItem.length === 0" class="btn btn-primary">
      Salvar Articulo
    </button>
  </form>
  <ul>
    <li 
     v-for="({ id, label, purchased, highPriority }, index) in items" 
      v-bind:key="id"
      :class="{strikeout: purchased, priority: highPriority}"
      @click="togglePurchased(items[index])"
      >
      🔹 {{ index }} {{ label }}
    </li>
  </ul>
  <p v-if="items.length === 0"> 📿Lista de compras vacia📿</p>
  <p v-else>🔥Ingrese mas items 🔥</p>
</template>
<style scoped>
.shopping-cart-icon {
  font-size: 2rem; /* Adjust the font-size value as per your desired size */
}
</style>