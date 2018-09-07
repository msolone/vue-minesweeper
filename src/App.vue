<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <section class="mine_sweeper">
      <MineSweeper />
    </section>
  </div>
</template>

<script>
import MineSweeper from './components/MineSweeper.vue'

export default {
  name: 'app',
  components: {
    MineSweeper
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
 
}

#app img {
  height: 12em;
  width: 12em;
}

.mine_sweeper {
  display: flex;
  justify-content: center;
}
</style>
