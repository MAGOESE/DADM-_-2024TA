<template>
    <div class="plan">
      <div class="description">
        <span class="title"> {{ planIcon }} {{ name }}
          
        </span>
      </div>
    </div>
  </template>
  
  <script setup>
  //Usando un macro para definir las propiedades
  const props = defineProps (['name','planIcon']);
  console.log(props.name);
  </script>
  
  <style scoped>
  /* Estilos específicos para el componente, si los necesitas */
  </style>
  