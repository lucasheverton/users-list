<template>
  <Home />
</template>

<script>
import Home from './components/Home.vue'

export default {
  name: 'App',
  components: {
    Home
  }
}
</script>

<style lang="scss">
  /* Variables */
$primary-background-color: #ffffff00;
$primary-color-text: #2c3e50;

html {
  background-color: $primary-background-color;
}

#app {
  font-family: 'Nunito', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: $primary-color-text;
  margin-top: 3em;
}
</style>
