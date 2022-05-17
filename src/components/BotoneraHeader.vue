<script>
import { Transition } from "vue"
import ModalLogin from "./modales/ModalLogin.vue"
import ModalProducto from "./modales/ModalProducto.vue"
import ModalCategoria from "./modales/ModalCategoria.vue"
export default {
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
    data() {
        return {
            logados: false,
            modalLogado: false,
            modalCategoria: false,
            modalProducto: false
        };
    },
    components: { ModalLogin ,ModalProducto, ModalCategoria}
}
</script>

<template>
    <div className='col-xs-12 col-md-3 col-sm-4'>
        <div className='row'>
            <Transition name="fade" mode="out-in">
                <input type='button' id='deslogarse' className='btn btn-primary deslogado' value='Deslogarse' v-if="logados" v-on:click="logados=false"/>
            </Transition>
            <Transition name="fade" mode="out-in">
                <input type='button' id='logarse' className='btn btn-primary deslogado' value='Logarse' v-if="!logados" v-on:click="modalLogado=true"/> 
            </Transition>
        </div>

        <Transition name="fade" mode="out-in">
            <div className='row mb-2' v-if="logados" >
                <input type='button' id='categoria' className='btn btn-primary deslogado creaciones' value='Crear Categoria' v-on:click="modalCategoria=true"/>
                <input type='button' id='producto' className='btn btn-primary deslogado creaciones' value='Crear Producto'  v-on:click="modalProducto=true"/>
            </div>
        </Transition>
    </div>
    <ModalLogin :visible="modalLogado" :fclose="()=>(modalLogado=false)" :flogados="()=>(logados=true)"/>
    <ModalProducto :visible="modalProducto" :fclose="()=>(modalProducto=false)" :grupocategorias="categorias" :agregarProducto="(categoria,producto)=>agregarProducto(categoria,producto)"/>
    <ModalCategoria :visible="modalCategoria" :fclose="()=>modalCategoria=false" :grupocategorias="categorias" :agregarCategoria="(nombre)=>agregarCategoria(nombre)"/>
</template>

<style>
    .fade-enter-active, .fade-leave-active {
        transition: opacity .5s
    }
    .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
        opacity: 0
    }
</style>
