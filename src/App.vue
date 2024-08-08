<template>
  <div id="app">
    <h1>Sopa de Letras</h1>
    <WordSearch />
  </div>
</template>

<script>
import WordSearch from './components/WordSearch.vue'

export default {
  name: 'App',
  components: {
    WordSearch
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #a6bfd4;
  margin-top: 60px;
}
</style>