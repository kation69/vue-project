<script>
export default {
    props: {
        idelemento:{
            type:String
        },
        visible:{
            type:Boolean
        },
        agotado:{
            type:Boolean
        },
        src_imagen:{
            type:String
        },
        datos: {
            type: Object
        },
        fclose:{
            type:Function
        },
        fcomprar:{
            type:Function
        }
    },
    methods:{
        mostrarPrecio(precio) {
            var coste = parseFloat(precio.replace(",", ".")).toFixed(2);
            if (coste == 0) {
                return "Gratis";
            }
            else {
                return coste.toString().replace(".", ",")+" €";
            }
        }
    }
}
</script>
<template> 
    <div className="modal productoMostrarModal" :id="idelemento" v-if="this.visible" tabIndex="-1" role="dialog" aria-labelledby="productoMostrarModalLabel" aria-hidden="true" @click="()=>this.fclose()">
        <div className="modal-dialog" role="document" @click="(e)=> e.stopPropagation()">
            <div className="modal-content">
            <div className="modal-header">
                <h3 className="modal-title" id="nombre_mostrar">{{datos.nombre}}</h3>
                <button type="button" className="close" data-dismiss="modal" id="CloseModal" aria-label="Close" @click="()=>this.fclose()">
                <span aria-hidden="true">×</span>
                </button>
            </div>
            <div className="modal-body">
                    <div className="row" align="center">
                        <div className="col-12">					
                            <form className="align-items-center pt-1 g-3 px-3">
                            <div className="form-group" id="group_categoria">
                                <h5 id="codigo_mostrar" class="mt-2">{{datos.codigo}}</h5>
                                <img :src="src_imagen" id="imagen_mostrar" className="img-thumbnail mw-95" />
                                <h3 id="precio_mostrar" class="mt-2">{{this.mostrarPrecio(datos.precio)}}</h3>
                            </div>
                            <div className="form-group py-4" id="descripcion">
                                <label htmlFor="productoLabel" className="form-label titulo_descripcion">Descripcion:</label>
                                <p id="descripcion_mostrar">{{datos.descripcion}}</p>
                            </div>
                            <div className="form-group py-4">
                                <Transition name="fade">
                                    <a v-show="!agotado" name="mostrar_restantes" className="btn btn-success text-white font-weight-bold" :aria-disabled="agotado" @click="()=>this.fcomprar()">
                                        <i className="bi bi-cart4 font-weight-bold"></i>
                                        <p className="Agregar_texto_oculto font-weight-bold">Agregar a la cesta</p>
                                    </a>
                                </Transition>
                                <Transition name="fade">
                                    <a v-show="agotado" className="btn acabado_mostrar text-white font-weight-bold" :aria-disabled="agotado">
                                        <i className="bi bi-cart-x-fill"></i>
                                        <p className="Agregar_texto_oculto font-weight-bold">Agregar a la cesta</p>
                                    </a>
                                </Transition>
                            </div>
                            </form>
                        </div>
                    </div>
                </div>
                <div className="modal-footer">
                    <button className="button" @click="()=>this.fclose()" >Cerrar</button>
                </div>
            </div>
        </div>
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