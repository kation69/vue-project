<script>
import ModalVista from "./modales/ModalVista.vue";
export default {
    props: {
        idelemento: {
            type: String
        },
        datos: {
            type: Object
        },
        fcomprar: {
            type: Function
        },
        sumarComprar: {
            type: Function
        }
    },
    data() {
        return {
            modalvista: false,
            agotado: false,
            claseagotado: "col-12 col-lg-4 col-xl-3 col-xxl-2 card producto rounder"
        };
    },
    methods: {
        recortar(texto, numero) {
            texto = texto.toString().trim();
            if (texto.length > numero) {
                texto = texto.substring(0, numero) + "...";
            }
            return texto;
        },
        mostrarPrecio(precio) {
            var coste = parseFloat(precio.replace(",", ".")).toFixed(2);
            if (coste == 0) {
                return "Gratis";
            }
            else {
                return coste.toString().replace(".", ",")+" €";
            }
        },
        comprobarAgotado(cantidad) {
            if (cantidad == "0") {
                this.claseagotado = "col-12 col-lg-4 col-xl-3 col-xxl-2 card producto acabado rounder";
                this.agotado = true;
            }
            else {
                this.claseagotado = "col-12 col-lg-4 col-xl-3 col-xxl-2 card producto rounder";
                this.agotado = false;
            }
            return cantidad;
        },
        comprar(elemento,cantidad) {
            if (cantidad != "0") {
                this.sumarComprar(elemento);
            }
        },
        imagen(src) {
            if (src.trim() === "") {
                return "./img/producto_neutro.jpeg";
            }
            else if (src.trim().startsWith("data:")) {
                console.log("producto creado");
                return src;
            }
            else {
                return "./img/" + src;
            }
        },
        idModal(idelemento) {
            return idelemento + "_modalview";
        }
    },
    components: { ModalVista }
}
</script>

<template>
  <div :id="idelemento"
    :class="this.claseagotado" >
    <div className="cuadrado card-img-bottom" @click="()=>(modalvista=true)">
      <img :src="imagen(datos.imagen)" className="rounded rellenoCuadrado float-botton" />
      <p className="bg-light text-success w-100 mt-1 position-absolute rounded-pill" name="codigo">{{datos.codigo}}</p>
    </div>
    <div className="card-body">
      <h3 className="card-title font-weight-bold">
        {{this.recortar(datos.nombre,17)}}</h3>
      <h5 className="card-title" name="precio" :value="datos.precio">{{this.mostrarPrecio(datos.precio)}}</h5>
      <p name="cantidad" :initial_value="datos.numero_unidades">Quedan {{comprobarAgotado(datos.numero_unidades)}}</p>
      <a name="mostrar_restantes" 
        @click="()=>(this.comprar(idelemento,datos.numero_unidades))"
        className="btn btn-success text-white font-weight-bold" :aria-disabled="this.agotado">
        <i className="font-weight-bold bi bi-cart4"></i> 
        <p className="font-weight-bold">Agregar a la cesta</p>
      </a>
      <p name="descripcion" >{{this.recortar(datos.descripcion,47)}}</p>
    </div>
  </div>
  <ModalVista :idelemento="this.idModal(idelemento)" :visible="modalvista"  :agotado="agotado" :fclose="()=>(modalvista=false)" :fcomprar="()=>(this.comprar(idelemento,datos.numero_unidades))" :datos="datos" :src_imagen="this.imagen(datos.imagen)"/>
</template>

<style scoped>

</style>
