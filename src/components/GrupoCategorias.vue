<script>
import Categorias from "./Categorias.vue";
export default {
    props: {
        categorias: {
            type: Object
        },
        sumarComprar: {
            type: Function
        }
    },
    methods: {
      tituloCategoria(nombre) {
        nombre=nombre.trim().toLowerCase();
        if(nombre.endsWith(".json")){
          nombre=nombre.substring(0,nombre.length-5);
        }
        return nombre[0].toUpperCase()+nombre.substring(1,nombre.length);
      }
    },
    components: { Categorias }
}
</script>

<template>
  <div id="productos" class="col-md-8 col-sm-12 col-12">
    <Categorias v-for="categoria in this.categorias" v-bind:key="tituloCategoria(categoria.nombre)" :sumarComprar="(idelemento)=>sumarComprar(idelemento)" :nombre="tituloCategoria(categoria.nombre)" :productos="categoria.datos" />
  </div>
</template>

<style scoped>

</style>
