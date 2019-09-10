<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
  // import './assets/jmessage-sdk-web.2.6.0.min.js'
  // import '../public/jmessage-sdk-web.2.6.0.min.js'

  export default {
  name: 'App'
}
</script>

<style lang="less">
.size{
  width: 100%;
  height: 100%;
}
html,body{
  .size;
  overflow: hidden;
  margin: 0;
  padding: 0;
}
#app {
  .size;
}
</style>
