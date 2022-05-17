<script>
export default {
    props: {
        comprados: {
            type: Object
        },
        fclose: {
            type: Function
        },
        eliminarListaComprar: {
            type: Function
        },
        visible: {
            type: Boolean
        }
    },
    data() {
        return {
            titulo:"Confirmar compra",
            texto_inicial: "",
            texto_foother:"",
            lista_comprada:false
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
        siguientePagina(){
            this.titulo="Compra confirmada";
            this.texto_foother=this.calculoTotal();
            this.lista_comprada=true;
        },
        cerrar(){
            this.fclose();
            if(this.lista_comprada){
                this.lista_comprada=false;
                this.eliminarListaComprar();
            }
            this.titulo="Confirmar compra";
            this.texto_foother="";
        },
        calculoTotal() {
            var total = 0;
            this.comprados.map((producto) => {
                total = total + (producto.cantidad * producto.precio);
                this.comprado = true;
            });
            console.log(total);
            if(total.toFixed(2)==0){
                return "La compra te salío GRATIS!!!"
            }
            return "Has gastado un total de "+total.toFixed(2).toString().replace(".",",")+" €.";
        }
    }
}
</script>
<template>
    <Transition name="fade">
    <div className="modal" v-if="visible" id="confirmacionModal" tabIndex="-1" role="dialog" aria-labelledby="confirmacionModalLabel" aria-hidden="true" @click="()=>this.cerrar()">
        <div className="modal-dialog" role="document" @click="(e)=> e.stopPropagation()">
            <div className="modal-content">
            <div className="modal-header">
                <h3 className="modal-title" id="confirmacionModalLabel">{{this.titulo}}</h3>
                <button type="button" className="close" data-dismiss="modal" id="CloseModal" aria-label="Close" @click="()=>this.cerrar()">
                    <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <div className="modal-body">
                    <div className="row" align="center">
                        <div className="col-12">					
                            <form className="align-items-center pt-1 g-3 px-3">
                                <div className="form-group mb-0">
                                    <label htmlFor="confirmLabel" id="confirmLabel" className="form-label">¿Estás seguro qué deseas realizar la compra?</label>
                                </div>
                                <Transition name="fade">
                                    <div className="form-group py-4 " v-if="!lista_comprada" id="botones_confirmar">
                                        <input type="button" className="btn btn-success mr-2" id="CompraConfirmada" value="Comprar" @click="()=>this.siguientePagina()"/>
                                        <input type="button" className="btn btn-danger" data-dismiss="modal"  id="CloseModal" value="Cancelar" aria-label="Close" @click="()=>this.cerrar()"/>
                                    </div>
                                </Transition>
                                <Transition name="fade">
                                    <div className="form-group py-4" v-if="lista_comprada" id="lista_comprada">
                                        <div v-for="producto in comprados" v-bind:key="producto.idelemento">
                                            <div id="_lista_compra" className="row">
                                                <div className="cuadrado_lista col-4 col-0 col-xl-4">
                                                    <img :src="this.imagen(producto.imagen)" className="rounded img-thumbnail mw-95 mh-100"/>
                                                </div>
                                                <div className="col-7 col-md-11 col-xl-7">
                                                    <h3 className="card-title">{{this.recortar(producto.nombre,17)}} - {{producto.codigo}}</h3>
                                                    <h4 name="precio">{{producto.cantidad}} X {{this.mostrarPrecio(producto.precio)}} €</h4>
                                                    <p>{{this.recortar(producto.descripcion,47)}}</p>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </Transition>
                            </form>
                        </div>
                    </div> 
                    <div className="modal-footer">
                        <h4 for="confirmLabel" class="modal-title" style="">{{this.texto_foother}}</h4>
                    </div>
                </div>
            </div>
        </div>
    </div>
    </Transition>
</template>
<style scoped>
  .fade-enter-active, .fade-leave-active {
        transition: opacity .25s
    }
    .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
        opacity: 0
    }
</style>