<template>
  <div id="root">
    <TheGame msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import TheGame from './components/TheGame.vue'

export default {
  name: 'App',
  components: {
    TheGame
  }
}
</script>
