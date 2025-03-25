<template>
  <div id="app">
    <h1>🛒 Web shop </h1>
    <Cart />
  </div>
</template>

<script setup>
import Cart from './components/Kosarica.vue';
</script>

<style scoped>
#app {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
  font-family: Arial, sans-serif;
  background-color: #f8f8f8;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
h1 {
  text-align: center;
  color: #333;
}
</style>

