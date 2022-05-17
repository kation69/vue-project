<script>
import Producto from "./Producto.vue";
export default {
    props: {
        nombre: {
            type: String
        },
        productos: {
            type: Object
        },
        sumarComprar: {
            type: Function
        }
    },
    data() {
        return {
            visible: true,
            claseVisible: "w-100 titulo_categoria font-weight-bold mt-3 p-3 pl-5 rounded-pill"
        };
    },
    methods: {
        cambiarVisible() {
            this.visible = this.visible == false;
            if (this.visible) {
                this.claseVisible = "w-100 titulo_categoria mt-3 p-3 pl-5 font-weight-bold rounded-pill";
            }
            else {
                this.claseVisible = "w-100 titulo_categoria mt-3 p-3 pl-5 font-weight-bold rounded-pill reducida";
            }
        },
        crearKey(nombre,index){
          return nombre+"_"+index.toString();
        }
    },
    components: { Producto }
}
</script>

<template>
  <div className="categoria row" 
    :id="nombre">
      <h2 :class="this.claseVisible" 
      @click="cambiarVisible()">{{nombre}}</h2>
      <Transition name="fade">
        <div v-if="this.visible" class="w-100">
          <Producto v-for="(elemento,index) in this.productos" v-bind:key="crearKey(nombre,index)" :idelemento="crearKey(nombre,index)" :datos="elemento" :sumarComprar="(idelemento)=>sumarComprar(idelemento)"/>
          <p v-if="this.productos.length==0" class="text-danger texto-grande">Sin productos en esta categoria</p>
        </div>
      </Transition>
    </div>
</template>

<style scoped>
  .fade-enter-active, .fade-leave-active {
        transition: opacity .75s
    }
    .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
        opacity: 0
    }
</style>
