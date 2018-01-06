<template>
  <div id="app">
    <small class="header-caution">Meiryoだとずれるのは仕様</small>
    <h1><img id="img-logo" src="./assets/logo.png"> Shinobi-View</h1>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>

<style>
#app {
  max-width: 1200px;
  font-family:/*  "MS Gothic", */ "Avenir", Helvetica, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-left: auto;
  margin-right: auto;
}

#img-logo {
  width: 40px;
  height: 40px;
}

.header-caution {
  font-size: 8px;
  opacity: 40%;
}
</style>
 