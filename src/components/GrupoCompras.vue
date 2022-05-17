<script>
import ProductoComprado from "./ProductoComprado.vue";
import ModalConfirmacion from "./modales/ModalConfirmacion.vue";
export default {
    props: {
        comprados: {
            type: Object
        },
        sumarComprar: {
            type: Function
        },
        eliminarComprar: {
            type: Function
        },
        eliminarListaComprar:{
            type: Function
        },
        restarComprar: {
            type: Function
        }
    },
    methods: {
        calculoTotal() {
            var total = 0;
            this.comprado = false;
            this.comprados.map((producto) => {
                total = total + (producto.cantidad * producto.precio);
                this.comprado = true;
            });
            return total.toFixed(2).toString().replace(".",",");
        },
        abrirModal() {
            this.visible = this.visible == false;
        },
        tituloCategoria(nombre) {
          nombre=nombre.trim().toLowerCase();
          if(nombre.endsWith(".json")){
            nombre=nombre.substring(0,nombre.length-5);
          }
          return nombre[0].toUpperCase()+nombre.substring(1,nombre.length);
        }
    },
    data() {
        return {
            comprado: false,
            visible: false
        };
    },
    components: { ProductoComprado, ModalConfirmacion }
}
</script>

<template>
  <div class="aspecto-carrito mt-3 col-md-4 col-sm-12 col-12">
        <div id="carrito" className="rounded h-100 p-2">
            <h2 className="rounded-pill font-weight-bold p-3 pl-5">Lista de la compra</h2>
            <div className="card rounded-pill">
                <h4 className="card-title m-3 text-dark">Total: <a id="total">{{calculoTotal()}}</a> € 
                <input type="button" id="confirmarCompra" className="btn btn-primary" defaultValue="Realizar pedido" :aria-disabled="!comprado" :disabled="!comprado" 
                @click="()=>abrirModal()" /></h4>
            </div>
            <ProductoComprado v-for="producto in comprados" v-bind:key="producto.id" :idelemento="producto.id" :datos="producto" :sumarComprar="(idelemento)=>sumarComprar(idelemento)"  :eliminarComprar="(idelemento)=>eliminarComprar(idelemento)" :restarComprar="(idelemento)=>restarComprar(idelemento)" :nombre="tituloCategoria(producto.nombre)"/>
        </div>
    </div>
    <ModalConfirmacion :visible="visible" :comprados="comprados" :eliminarListaComprar="()=>eliminarListaComprar()" :fclose="()=>visible=false"/>
</template>

<style scoped>

</style>
