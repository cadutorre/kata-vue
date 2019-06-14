<template>
  <div id="app">
    <game-of-life></game-of-life>
  </div>
</template>

<script>
import GameOfLife from "@/components/GameOfLife.vue"
export default {
  components: { GameOfLife }
}
</script>


<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
