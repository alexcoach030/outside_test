<template>
  <div id="app">
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
body{
  margin: 0;
  padding: 0;
}
#app {
  background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0 auto;
  font-family: Lab G, sans-serif;
  display: flex;
  justify-content: space-around;
  height: 100vh;
  width: 100vw;
}
</style>
