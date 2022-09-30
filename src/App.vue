<template>
  <v-app>
    <div class="main__app">
      <div class="main__banner">

      </div>
      <productos/>
    </div>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';
import Productos from './components/Productos.vue';

export default {
  name: 'App',

  components: {
    // HelloWorld
    Productos,
  },

  data: () => ({
    //
  }),
};
</script>
<style>
html{
  /* background-color: rgb(103, 103, 103); */
  
}
.main__app{
  width: 100vw;
  height: 100vh;
  background-color: rgb(231, 231, 231);
}
</style>
