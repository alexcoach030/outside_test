<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <PopupComponent/>
  </div>
</template>

<script>

import PopupComponent from "@/components/PopupComponent.vue";

export default {
  name: 'App',
  components: {
    PopupComponent
  }
}
</script>

<style lang="less">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
