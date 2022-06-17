<script setup lang="ts">
import { RouterView } from 'vue-router'
</script>

<template>
  <header>
    <h1>Тестовое задание: Компонент <b>«Folder explorer»</b> </h1>
  </header>
  <main>
   <RouterView />
  </main>
</template>

<style>
@import '@/assets/base.css';

#app {
  display: grid;
  grid-template-columns: var(--grid-template-columns);
  grid-template-rows: 100px 1fr;
  margin: 0 auto;
  font-weight: normal;
}

#app header {
  grid-area: 1 / 1 / 2 / 4;
  display: grid;
  background-color: var(--vt-c-indigo);
  align-items: center;
  color: var(--color-background);
  line-height: 1.5;
  max-height: 100vh;
}

header > h1 {
  grid-column: 2
}

main {
  grid-area: 2 / 2 / 3 / 3;
}

a {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}


</style>
