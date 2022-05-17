<script>
import GrupoCategorias from "./GrupoCategorias.vue";
import GrupoCompras from "./GrupoCompras.vue";

export default {
    props: {
        categorias: {
            type: Object
        },comprados: {
            type: Object
        },sumarComprar: {
            type: Function
        },
        eliminarComprar: {
            type: Function
        },
        eliminarListaComprar: {
            type: Function
        },
        restarComprar: {
            type: Function
        }
    },
    components: { GrupoCategorias, GrupoCompras }
}
</script>

<template>
  <div className="container-big container">
    <div className="row">
      <GrupoCategorias :categorias="categorias" :sumarComprar="(idelemento)=>sumarComprar(idelemento)"/>
      <GrupoCompras :comprados="comprados"  :sumarComprar="(idelemento)=>sumarComprar(idelemento)"  :eliminarComprar="(idelemento)=>eliminarComprar(idelemento)" :eliminarListaComprar="()=>eliminarListaComprar()" :restarComprar="(idelemento)=>restarComprar(idelemento)" />
    </div>
  </div>
</template>

<style scoped>

</style>
