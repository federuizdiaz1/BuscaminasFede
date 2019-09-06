<template>
  <div class="principal"> <!-- definir elemento principal -->
   <tablero />
  </div>
</template>
 <!-- importamos el componente tablero -->
<script>
import Tablero from './components/Tablero.vue'

export default {
  name: 'app',
  components: {
    Tablero
  }
}
</script>
 <!-- definimos para el elemento principal tipo grilla-->
<style>
.principal{
  display:grid;
  justify-content:center;
}

</style>
