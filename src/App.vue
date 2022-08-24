<template>
  <div id="app">
    <div class="container">
      <router-view />
    </div>
  </div>
</template>

<script>
import 'bulma/css/bulma.min.css';

export default {

}
</script>

<style scoped>
  body {
    background: #475569;
  }

  #app {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translateY(-50%) translateX(-50%);
  }
</style>