<script setup>
import GrupoCabecera from './components/GrupoCabecera.vue';
import GrupoCuerpo from './components/GrupoCuerpo.vue';
import PiePagina from './components/PiePagina.vue';
import grupos from './juegodatos';
</script>
<script>
export default {
    data() {
        return {
            categorias:this.cabeceras(grupos()),
            datos:grupos(),
            comprado:[]
            
        };
    },
    methods:{
      cabeceras(datos){
        var categorias=[];
        datos.map((categoria)=>{
          if(categoria.nombre.toUpperCase().trim().endsWith(".JSON")){
            categorias.push(categoria.nombre.toUpperCase().trim().substring(0,categoria.nombre.toUpperCase().trim().length-5));
          }
          else{
            categorias.push(categoria.nombre.toUpperCase().trim());
          }
        });
        return categorias;
      },
      sumarComprar(idelemento){
        var existe=false;
        this.comprado.map((elemento)=>{
          if(elemento.idelemento==idelemento.toUpperCase()){
            if(parseFloat(elemento.maximo).toFixed(0)>elemento.cantidad){
              elemento.cantidad=elemento.cantidad+1;
            }
            existe=true;
          }
        });
        var elemento={};
        var codigo="";
        this.datos.map((categoria)=>{
          categoria.datos.map((producto,index)=>{
            var nombre="";
            if(categoria.nombre.toUpperCase().trim().endsWith(".JSON")){
              nombre=categoria.nombre.toUpperCase().trim().substring(0,categoria.nombre.toUpperCase().trim().length-5);
            }
            else{
              nombre=categoria.nombre.toUpperCase().trim();
            }
            if(nombre+"_"+index.toString()==idelemento.toUpperCase()){
              elemento["maximo"]=parseFloat(producto.numero_unidades).toFixed(0);
              producto.numero_unidades=(parseFloat(producto.numero_unidades).toFixed(0)-1).toString();
              elemento["descripcion"]=producto.descripcion.toString();
              elemento["codigo"]=producto.codigo.toString();
              elemento["nombre"]=producto.nombre.toString();
              elemento["imagen"]=producto.imagen.toString();
              elemento["precio"]=parseFloat(producto.precio.replace(",",".")).toFixed(2);
            }
          });
        });
        if(!existe){
          elemento["cantidad"]=1;
          elemento["idelemento"]=idelemento.toUpperCase();
          elemento["id"]=idelemento.toUpperCase()+"_comprado";
          this.comprado.push(elemento);
        }
      },
      restarComprar(idelemento){
        this.comprado.map((elemento,index)=>{
          if(elemento.idelemento.toUpperCase()==idelemento.toUpperCase()){
            if(1<elemento.cantidad){
              elemento.cantidad=elemento.cantidad-1;
            }
          }
        });
        this.datos.map((categoria)=>{
          categoria.datos.map((producto,index)=>{
            var nombre="";
            if(categoria.nombre.toUpperCase().trim().endsWith(".JSON")){
              nombre=categoria.nombre.toUpperCase().trim().substring(0,categoria.nombre.toUpperCase().trim().length-5);
            }
            else{
              nombre=categoria.nombre.toUpperCase().trim();
            }
            if(nombre+"_"+index.toString()==idelemento.toUpperCase()){
              producto.numero_unidades=((parseFloat(producto.numero_unidades)+1).toFixed(0)).toString();
            }
          });
        });
      },
      eliminarComprar(idelemento){
        var num_index=-1;
        var cantidad=0;
        this.comprado.map((elemento,index)=>{
          if(elemento.idelemento.toUpperCase()==idelemento.toUpperCase()){
            console.log(idelemento);
            console.log(elemento.idelemento);
            num_index=index;
            cantidad=elemento.maximo;
          }
        });
        if(num_index!=-1){
          this.comprado.splice( num_index,1);
        }
        this.datos.map((categoria)=>{
          categoria.datos.map((producto,index)=>{
            var nombre="";
            if(categoria.nombre.toUpperCase().trim().endsWith(".JSON")){
              nombre=categoria.nombre.toUpperCase().trim().substring(0,categoria.nombre.toUpperCase().trim().length-5);
            }
            else{
              nombre=categoria.nombre.toUpperCase().trim();
            }
            if(nombre+"_"+index.toString()==idelemento.toUpperCase()){
              producto.numero_unidades=cantidad.toString();
            }
          });
        });
      },
      eliminarListaComprar(){
        this.comprado=[];
      },
      agregarCategoria(nombre){
        console.log(nombre);
        this.categorias.push(nombre.toUpperCase());
        this.datos.push({nombre:nombre,datos:[]});
      },
      agregarProducto(categoria,elemento){
        this.datos.map((categorias)=>{
          var nomb_cate="";
          if(categorias.nombre.trim().toUpperCase().endsWith(".JSON")){
            nomb_cate=categorias.nombre.trim().toUpperCase().substring(0,categorias.nombre.trim().length-5);
          }
          else{
            nomb_cate=categorias.nombre.trim().toUpperCase();
          }
          if(nomb_cate==categoria.toUpperCase()){
            categorias.datos.push(elemento);
          }
        });
      }

    }
}
</script>


<template>
  <main>
    <GrupoCabecera :categorias="categorias" :agregarCategoria="(nombre)=>agregarCategoria(nombre)" :agregarProducto="(categoria,producto)=>agregarProducto(categoria,producto)" />
    <GrupoCuerpo :categorias="datos" :comprados="comprado" :sumarComprar="(idelemento)=>sumarComprar(idelemento)"  :eliminarComprar="(idelemento)=>eliminarComprar(idelemento)" :eliminarListaComprar="()=>eliminarListaComprar()" :restarComprar="(idelemento)=>restarComprar(idelemento)"/>
    <PiePagina/>
  </main>
</template>

<style>
@import './assets/base.css';
@import './assets/index.css';

</style>
