<script setup>
import { ref, computed } from 'vue';

// Modelo
const header = ref('App lista de compras');
const items = ref([ 
  { id: '0', label: '10 Bolillos', purchased: false, priority: false },
  { id: '1', label: '1 Lata Frijoles', purchased: false, priority: false },
  { id: '2', label: '1 Chelas', purchased: false, priority: false },
  { id: '3', label: '1 Nutella', purchased: false, priority: true },
]);

// Visualización de formulario
const newItem = ref('');
const newItemHighPriority = ref(false);
const editing = ref(false); // Empezamos con el formulario oculto

const ActivateEdition = (activate) => {
  editing.value = activate;
};

// Método para guardar item
const saveItem = () => {
  items.value.push({
    id: `${items.value.length}`, // Mantener la estructura de id como cadena
    label: newItem.value,
    purchased: false,
    priority: newItemHighPriority.value
  });

  // Limpiar la entrada
  newItem.value = '';
  newItemHighPriority.value = false; // Limpiar también la prioridad

  // Desactivar el modo de edición después de guardar
  editing.value = false;
};

// Propiedad computada para contar los caracteres en la entrada
const characterCount = computed(() => {
  return newItem.value.length;
});

// Propiedad computada para invertir el orden de los elementos en la lista
const reversedItems = computed(() => {
  return [...items.value].reverse();
});
</script>

<template>
  <div class="header">
    <h1>
      <i class="material-icons shopping-cart-icon">local_mall</i>
      {{ header }}
    </h1>
    <button v-if="editing" class="btn" @click="ActivateEdition(false)">
      Cancelar
    </button>
    <button v-else class="btn" @click="ActivateEdition(true)">
      Agregar Articulo
    </button>
  </div>

  <div class="add-item form">
    <!-- Agrupando en un div las entradas -->
    <form 
      v-if="editing"
      v-on:submit.prevent="saveItem">
      <!-- Entrada de texto --> 
      <input
        v-model.trim="newItem"
        type="text"
        placeholder="Add Item"
      />
      <!-- Contador de caracteres -->
      <p class="counter">
        {{ characterCount }} / 200
      </p>
      <!-- Caja de seleccion de prioridad -->
      <label>
        <input type="checkbox" v-model="newItemHighPriority" />
        High Priority
      </label>
      <!-- Botón -->
      <button class="btn btn-primary">
        Save Item
      </button>
    </form>
  </div>

  <!-- Lista -->
  <ul>
    <li 
      v-for="{ id, label, purchased, priority } in reversedItems" 
      v-bind:key="id"
      class="amazing"
      v-bind:class="{ priority: priority, strikeout: purchased }"> 
      {{ priority ? "🔥" : "🔹" }} {{ label }}
    </li>
  </ul>
  <p v-if="items.length === 0">🥀 AQUI NO HAY NADA 😿</p>
</template>

<style scoped>
.shopping-cart-icon {
  font-size: 2rem;
}
.counter {
  font-size: 0.9rem;
  color: #888;
}
</style>
