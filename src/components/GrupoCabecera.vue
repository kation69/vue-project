<script>
  import TituloPagina from './TituloPagina.vue';
  import BotoneraHeader from './BotoneraHeader.vue';
  export default{
    props:{
      categorias:{
        type:Object
      },
      agregarCategoria:{
          type:Function
      },
      agregarProducto:{
          type:Function
      }
    },
    components: { TituloPagina ,BotoneraHeader}
  }
</script>

<template>
  <div className='grupo_titulos w-100'>
      <TituloPagina/>
      <BotoneraHeader :categorias="this.categorias" :agregarCategoria="(nombre)=>agregarCategoria(nombre)" :agregarProducto="(categoria,producto)=>agregarProducto(categoria,producto)"/>
  </div>
</template>

<style scoped>

</style>
