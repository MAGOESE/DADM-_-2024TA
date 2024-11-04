<script setup>
import { ref, computed } from 'vue';

// Modelo
const header = ref('App lista de compras');
const items = ref([
  { id: 0, label: '10 Bolillos', purchased: false, priority: false },
  { id: 1, label: '1 Lata Frijoles', purchased: false, priority: false },
  { id: 2, label: '1 Chelas', purchased: false, priority: false },
  { id: 3, label: '1 Nutella', purchased: false, priority: true },
]);

// Visualización de formulario
const newItem = ref('');
const newItemHighPriority = ref(false);
const editing = ref(false); // Empezamos con el formulario oculto

const ActivateEdition = (activate) => {
  editing.value = activate;
};

// Método para guardar un nuevo item
const saveItem = () => {
  items.value.push({
    id: items.value.length,
    label: newItem.value,
    purchased: false,
    priority: newItemHighPriority.value,
  });

  // Limpiar la entrada
  newItem.value = '';
  newItemHighPriority.value = false;
  editing.value = false; // Desactivar el modo de edición después de guardar
};

// Alternar estado de compra del item
const togglePurchased = (item) => {
  item.purchased = !item.purchased;
};

// Propiedad computada: Contador de caracteres
const characterCount = computed(() => {
  return newItem.value.length;
});

// Propiedad computada: Lista invertida
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
    <form v-if="editing" @submit.prevent="saveItem">
      <input v-model.trim="newItem" type="text" placeholder="Add Item" />
      <label>
        <input type="checkbox" v-model="newItemHighPriority" />
        High Priority
      </label>
      <button class="btn btn-primary">Save Item</button>
      <p class="counter">{{ characterCount }} / 200</p>
    </form>
  </div>

  <!-- Lista -->
  <ul>
    <li
      v-for="({ id, label, purchased, priority }, index) in reversedItems"
      :key="id"
      :class="{ strikeout: purchased, priority: priority }"
      @click="togglePurchased(reversedItems[index])"
    >
      {{ priority ? '🔥' : '🔹' }} {{ label }}
    </li>
  </ul>
  <p v-if="items.length === 0">🥀 No hay elementos en la lista</p>
</template>

<style scoped>
.shopping-cart-icon {
  font-size: 2rem;
}
.strikeout {
  text-decoration: line-through;
}
.priority {
  color: red;
  font-weight: bold;
}
</style>
