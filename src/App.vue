<template>
  <v-app>
    <v-content>
      <SearchBar></SearchBar>
    </v-content>
  </v-app>

</template>

<script>
import SearchBar from './components/SearchBar.vue'

export default {
  name: 'app',
  components: {
    SearchBar
  }
}
</script>

<style>
#app {
  background: linear-gradient(141deg, #0fb8ad 0%, #1fc8db 51%, #2cb5e8 75%);
}
</style>
