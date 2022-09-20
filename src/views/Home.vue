<template>
  <Banner/>
  <div class="main-container">
    <h1>Monte o seu lanche:</h1>
    <FormularioBurger/>
  </div>
  <Footer/>
</template>

<script>
import Banner from '../components/Banner.vue';
import FormularioBurger from '../components/FormularioBurger.vue';
import Footer from '../components/Footer.vue';
export default {
  name: 'Home',
  components: {
    Banner,
    FormularioBurger,
    Footer
}
}
</script>