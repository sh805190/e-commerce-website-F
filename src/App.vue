<template>
  <div id="app">
    <div class="parent">
      <!-- <div class="content"><p>Hello!</p></div> -->
    </div>
    <!-- <div id="nav">
      <router-link to="/">Home</router-link>
      <router-link to="/about">About</router-link>
    </div> -->

    <router-view />

  </div>
</template>

<script>
  export default {
    name: "App",

  };
</script>

<style lang="scss">
  @import "./assets/all";

  .content {
    padding: 70px 0;
    text-align: center;
  }

</style>
