<template>
  <number-guesser>
  </number-guesser>
</template>

<script>
import NumberGuesser from './components/NumberGuesser.vue'

export default {
  name: 'App',
  components: {
    NumberGuesser
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
