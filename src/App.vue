<template>
  <Navigation />
  <router-view/>
</template>

<script>
import Navigation from './components/Navigation.vue'

export default {
  components: {
    Navigation
  }
}
</script>

<style lang="scss">
@import "./sass/globals";



</style>
