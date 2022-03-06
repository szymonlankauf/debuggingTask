<template>
  <div id="app">
    <PokeFetcher />
  </div>
</template>

<script>
import PokeFetcher from './components/PokeFetcher.vue'

export default {
  name: 'App',
  components: {
    PokeFetcher,
  }
}
</script>

<style>
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.poke-btn {
  box-shadow: 0px 8px 22px -6px rgba(66, 68, 90, 1);
  padding: 4px 24px;
  background: #4895ef;
  color: white;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}
.poke-btn:hover {
  transform: translateY(-2px);
  transition: all .2s;
  box-shadow: 0px 8px 22px -4px rgba(66, 68, 90, 1);
}
.poke-btn:active {
  transform: translateY(-1px);
}
.poke-btn:active, .poke-btn:focus {
  outline: 1px solid #012a4a;
}
.poke-btn--disabled {
  box-shadow: none;
  background: #ced4da;
  cursor: not-allowed;
}
.poke-btn--disabled:hover {
  transform: none;
  box-shadow: none;
}
</style>
