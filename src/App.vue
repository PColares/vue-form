<script setup>
import { RouterView } from 'vue-router'
</script>

<template>
  <div>
    <!-- <header>
      <nav>
        <RouterLink to="/">Home</RouterLink>
      </nav>
    </header> -->
    <RouterView />
  </div>
</template>


<style>
  #app {
    max-width: 450px;
    margin: 60px auto;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
  }
</style>
