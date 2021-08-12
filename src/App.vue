<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <CwAutocomplete :model="model" :list="list" />
  </div>
</template>

<script>
import CwAutocomplete from './components/autocomplete'

export default {
  components: {
    CwAutocomplete
  },
  data() {
    return {
      list: ['a', 'aaaa', 'abbb', 'cccc', 'cdddd', 'eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee', 'f'],
      model: ''
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
