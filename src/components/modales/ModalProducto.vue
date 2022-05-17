<script>
export default{
  props:{
    grupocategorias:{
      type:Object
    },
    visible: {
      type: Boolean,
      required: true
    },
    fclose:{
      type:Function
    },
    agregarProducto:{
      type:Function
    }
  },
  data(){
    return(
      {
        categoria:"",
        nombre:"",
        codigo:"",
        precio:0,
        cantidad:0,
        descripcion:"",
        src_imgen:"./img/producto_neutro.jpeg",
        error_categoria:"form-control border",
        error_nombre:"form-control border",
        error_codigo:"form-control border",
        error_precio:"form-control border",
        error_cantidad:"form-control border",
        error_descripcion:"form-control border"
      }
    )
  },
  methods:{
    cerrar(){
      this.error_categoria="form-control border";
      this.error_nombre="form-control border";
      this.error_codigo="form-control border";
      this.error_precio="form-control border";
      this.error_cantidad="form-control border";
      this.error_descripcion="form-control border";
      this.categoria="";
      this.nombre="";
      this.codigo="";
      this.src_imgen="./img/producto_neutro.jpeg",
      this.precio=0;
      this.cantidad=0;
      this.descripcion="";
      this.fclose();
    },
    tieneErrores(texto){
      return texto.includes("danger");
    },
    comprobarCrearProducto(){
      var elemento={};
      if(this.categoria.trim()==""){
        this.error_categoria="form-control border border-danger";
      }
      else{
        this.error_categoria="form-control border";
      }
      if(this.nombre.trim()==""){
        this.error_nombre="form-control border border-danger";
      }
      else{
        this.error_nombre="form-control border";
        elemento["nombre"]=this.nombre.trim();
      }
      if(this.codigo.trim()==""){
        this.error_codigo="form-control border border-danger";
      }
      else{
        this.error_codigo="form-control border";
        elemento["codigo"]=this.codigo.trim();
      }
      if(this.descripcion.trim()==""){
        this.error_descripcion="form-control border border-danger";
      }
      else{
        this.error_descripcion="form-control border";
        elemento["descripcion"]=this.descripcion.trim();
      }
      if(this.precio<0 || parseFloat(this.precio).toFixed(2)!=this.precio){
        this.error_precio="form-control border border-danger";
      }
      else{
        this.error_precio="form-control border";
        elemento["precio"]=this.precio.toString();
      }
      if(this.cantidad<0 || parseFloat(this.cantidad).toFixed(0)!=this.cantidad){
        this.error_cantidad="form-control border border-danger";
      }
      else{
        this.error_cantidad="form-control border";
        elemento["numero_unidades"]=this.cantidad.toString();
      }
      //Errores?
      if(this.error_categoria=="form-control border" && this.error_nombre=="form-control border" && this.error_codigo=="form-control border" && this.error_precio=="form-control border" && this.error_cantidad=="form-control border" && this.error_descripcion=="form-control border"){
        if(document.getElementById("new_producto_imagen").value.length!=0){
          elemento["imagen"]=document.getElementById('imagen_a_mostrar').src;
        }
        else{
          elemento["imagen"]="producto_neutro.jpeg";
        }
        
        this.agregarProducto(this.categoria.trim(),elemento);
        this.cerrar();
      }
      else{
        console.log("errores");
      }
    },
    imagenCargada(e){
      var files = e.target.files || e.dataTransfer.files;
      if (files.length===0){
        console.log("limpia");
        document.getElementById('imagen_a_mostrar').src="./img/producto_neutro.jpeg";
        return;
      }
      var reader = new FileReader();
      
      reader.readAsDataURL(files[0]);

      reader.onload = function(){
          document.getElementById('imagen_a_mostrar').src=reader.result;
          return;
      }
    }
  }
}
</script>
<template>
  <Transition name="fade">
    <div className="modal" v-if="this.visible" id="productoModal" tabIndex="-1" role="dialog" aria-labelledby="productoModalLabel" aria-hidden="true"  @click="()=>cerrar()">
      <div className="modal-dialog" role="document" @click="(e)=> e.stopPropagation()">
        <div className="modal-content">
          <div className="modal-header">
            <h3 className="modal-title" id="productoModalLabel">Crear producto</h3>
            <button type="button" className="close" data-dismiss="modal" id="CloseModal" aria-label="Close" @click="()=>cerrar()">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div className="modal-body">
          <div className="row" align="center">
            <div className="col-12">					
              <form className="align-items-center pt-1 g-3 px-3">
                <div className="form-group" id="group_categoria">
                  <h4>Nuevo Producto</h4>
                </div>
                <div className="form-group" id="group_producto">
                  <label htmlFor="productoLabel" className="form-label">Categoria</label>
                  <select v-model="categoria" :class="error_categoria" id="new_producto_categoria" name="new_producto_categoria" alt="Categoria">
                    <option value="">Seleccione categoria...</option>
                    <option v-for="categoria in grupocategorias" v-bind:key="categoria"  v-bind:value="categoria">{{categoria}}</option>
                  </select>
                  <Transition name="fade">
                    <p className="text-center texto_error" v-show="tieneErrores(error_categoria)" id="error_vacia_categoria_producto">Seleccione categoria valida</p>
                  </Transition>
                </div>
                <div className="form-group" id="group_producto">
                  <label htmlFor="productoLabel" className="form-label">Nombre</label>
                  <input type="text" v-model="nombre" :class="error_nombre" id="new_producto_nombre" name="new_producto_nombre" alt="Nombre" placeholder="Escriba el nombre del producto" />
                  <Transition name="fade">
                    <p className="text-center texto_error" v-show="tieneErrores(error_nombre)" id="error_vacia_producto_nombre">Indique nombre del producto</p>
                  </Transition>
                </div>
                <div className="form-group" id="group_producto">
                  <label htmlFor="productoLabel" className="form-label">Codigo</label>
                  <input type="text" v-model="codigo" :class="error_codigo" id="new_producto_codigo" name="new_producto_codigo" alt="Codigo" placeholder="Escriba el codigo del producto" />
                  <Transition name="fade">
                    <p className="text-center texto_error" v-show="tieneErrores(error_codigo)" id="error_vacia_producto_codigo">Indique codigo del producto</p>
                  </Transition>
                </div>
                <div className="form-group" id="group_producto">
                  <label htmlFor="productoLabel" className="form-label">Precio (€)</label>
                  <input type="number" v-model="precio" :class="error_precio" id="new_producto_precio" name="new_producto_precio" alt="Precio" defaultValue="0" min="0" step="0.01"  placeholder="Escriba el precio del producto" />
                  <Transition name="fade">
                    <p className="text-center texto_error" v-show="tieneErrores(error_precio)" id="error_vacia_producto_precio">Indique precio adecuado al producto</p>
                  </Transition>
                </div>
                <div className="form-group" id="group_producto">
                  <label htmlFor="productoLabel" className="form-label">Descripcion</label>
                  <textarea type="text" v-model="descripcion" :class="error_descripcion" id="new_producto_descripcion" name="new_producto_descripcion" alt="Descripcion" placeholder="Escriba la descripcion del producto"></textarea>
                  <Transition name="fade">
                    <p className="text-center texto_error" v-show="tieneErrores(error_descripcion)" id="error_vacia_producto_descripcion">Indique descripcion del producto</p>
                  </Transition>
                </div>
                <div className="form-group" id="group_producto">
                  <label htmlFor="productoLabel" className="form-label">Imagen</label>
                  <input type="file" className="form-control border" id="new_producto_imagen" name="new_producto_imagen" alt="imagen" placeholder="Escriba la ruta de la imagen del producto" 
                  @change="(e)=>this.imagenCargada(e)" />
                  <img :src="src_imgen" id="imagen_a_mostrar" className="mw-95 mt-3" />
                </div>
                <div className="form-group" id="group_producto">
                  <label htmlFor="productoLabel" className="form-label">Numero de unidades</label>
                  <input type="number" v-model="cantidad" step="1" :class="error_cantidad" id="new_producto_numero_unidades" name="new_producto_numero_unidades" alt="Cantidad" min="0"  placeholder="Escriba el cantidad de producto" />
                  <Transition name="fade">
                    <p className="text-center texto_error" v-show="tieneErrores(error_cantidad)" id="error_vacia_producto_numero_unidades">Indique numero de unidades adecuado al producto</p>
                  </Transition>
                </div>
                <div className="form-group py-4">
                  <input type="button" className="btn btn-primary" id="crearProducto" @click="comprobarCrearProducto()" value="Crear producto" />
                </div>
              </form>
            </div>
          </div>
        </div>
        <div className="modal-footer">
          <button className="button" @click="()=>cerrar()" >Cerrar</button>
        </div>
        </div>
      </div>
    </div>
  </Transition> 
</template>

<style scoped>
  .fade-enter-active, .fade-leave-active {
      transition: opacity .75s
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
      opacity: 0
  }
</style>