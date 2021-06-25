<template lang="">

  <div>
    <Navbar />
    <div class="container-fluid">


      <div div class="row">
        <Sidebar />
        <main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
          <router-view />
        </main>

      </div>
    </div>

  </div>
</template>
<script>
  import Sidebar from "./Sidebar";
  import Navbar from "./Navbar.vue";
  export default {
    components: {
      Sidebar,
      Navbar,
    },

    methods: {

    },
  };
</script>
<style lang="">
</style>
