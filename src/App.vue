<template>
  <img alt="Ehterum logo" src="https://ethereum.org/static/a183661dd70e0e5c70689a0ec95ef0ba/cdbe4/eth-diamond-purple.webp">
  <h1>ether-yum-ui</h1>
  <ContractSearch />
</template>

<script>
import ContractSearch from './components/ContractSearch.vue'

export default {
  name: 'App',
  components: {
    ContractSearch
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
