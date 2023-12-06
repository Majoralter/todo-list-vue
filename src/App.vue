<template>
  <Layout />
</template>

<script>
import './globals.scss'
import Layout from './components/Layout.vue';

export default {
  name: 'App',
  components: {
    Layout
  }
}
</script>

<style lang="scss">
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
