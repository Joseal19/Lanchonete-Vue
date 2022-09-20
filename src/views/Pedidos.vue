<template>
  <div class="main-container">
    <h1>Gerenciar pedidos:</h1>
    <Dashboard />
    <Footer/>
  </div>
</template>

<script>
  import Dashboard from '../components/Dashboard.vue';
  import Footer from '../components/Footer.vue';
  export default {
      name: "Pedidos",
      components: { 
        Dashboard,
        Footer
      }
  }
</script>

<style scoped>
  #footer{
    bottom: 0;
    left: 0;
  }
</style>