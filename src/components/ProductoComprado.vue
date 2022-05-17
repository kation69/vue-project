<script>
import ModalVista from "./modales/ModalVista.vue";
export default {
    props: {
        nombre: {
            type: String
        },
        idelemento: {
            type: String
        },
        datos: {
            type: Object
        },
        fcomprar: {
            type: Function
        },
        eliminarComprar: {
            type: Function
        },
        restarComprar: {
            type: Function
        },
        sumarComprar: {
            type: Function
        }
    },
    data() {
        return {
            modalvista: false,
            acabado:false,
            cantidad_comprada:0
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
        mostrarPrecio(precio,cantidad) {
            var coste = parseFloat(precio.replace(",", ".")).toFixed(2);
            this.cantidad_comprada=cantidad;
            if (coste == 0) {
                return "Gratis";
            }
            else {
                return coste.toString().replace(".", ",")+" €";
            }
        },
        comprobarAgotado(cantidad,maximo) {
            if (cantidad ==parseFloat(maximo).toFixed(0)) {
                return true;
            }
            else {
                return false;
            }
        },
        cambio(inputs){
            var valor = inputs.target.value;
            if(valor>this.datos.maximo){
                valor=this.datos.maximo;
            }
            else if(valor<1){
                valor=1;
            }
            if(this.datos.cantidad!=valor){
                valor=valor-this.datos.cantidad;
                console.log(this.datos);
                if(valor>0){
                    for(var i=0;i<valor;i++){
                        this.comprar(this.datos.idelemento,parseFloat(this.datos.cantidad).toFixed(0)+i,parseFloat(this.datos.maximo).toFixed(0));
                    }
                }
                else if(valor<0){
                    for(var i=0;i<-valor;i++){
                        this.restar(this.datos.idelemento,parseFloat(this.datos.cantidad).toFixed(0)-i);
                    }
                }
            }
            inputs.target.value=valor;
            
        },
        comprar(elemento,cantidad,maximo) {
            if (maximo!=cantidad) {
                this.sumarComprar(elemento);
            }
        },
        restar(elemento,cantidad) {
            if (cantidad !=1) {
                this.restarComprar(elemento);
            }
        },
        eliminar(elemento) {
            this.eliminarComprar(elemento);
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
            return idelemento + "_modalviewcomprado";
        }
    },
    components: { ModalVista }
}
</script>

<template>
    <div :id="idelemento" className="row col-12 producto_carrito text-dark">
        <div className="cuadrado_lista col-4 col-0 col-xl-4">
          <img :src="imagen(datos.imagen)" className="rounded img-thumbnail rellenoCuadradoLista mw-95 mh-100 cursor_help position-relative" 
          @click="()=>(modalvista=true)"/>
        </div>
        <div className="col-7 col-md-11 col-xl-7">
          <h3 className="card-title">{{recortar(datos.nombre,17)+" - "+datos.codigo}}</h3>
          <h4 name="precio">{{mostrarPrecio(datos.precio,datos.cantidad)}}</h4>
          <p>{{recortar(datos.descripcion,47)}}</p>
          <div className="decoreinput">
            <input className="text-dark" type="number" step="1" min="1" :max="datos.maximo" :value="cantidad_comprada" @change="(e)=>this.cambio(e)"/>
            <div className="decoreinput-nav">
              <div className="decoreinput-button decoreinput-up" @click="()=>(this.comprar(datos.idelemento,datos.cantidad,datos.maximo))">+</div>
              <div className="decoreinput-button decoreinput-down" @click="()=>(this.restar(datos.idelemento,datos.cantidad))">-</div>
            </div>
          </div>
          <a className="btn btn-warning" @click="()=>(eliminar(datos.idelemento))">
            <i className="bi bi-x-circle-fill"></i>
            <p className="Agregar_texto_oculto"> Eliminar de la lista</p>
          </a>
        </div>
      </div>
      <br/>
  <ModalVista :idelemento="this.idModal(idelemento)" :visible="modalvista"  :agotado="comprobarAgotado(datos.cantidad,datos.maximo)" :fclose="()=>(modalvista=false)" :fcomprar="()=>(this.comprar(datos.idelemento,datos.cantidad,datos.maximo))" :datos="datos" :src_imagen="this.imagen(datos.imagen)"/>
</template>

<style scoped>

</style>
