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
    agregarCategoria:{
        type:Function
    }
  },
  data(){
    return(
      {
        categoria:"",
        claseerror:"form-control border",
        vacio:false,
        error:false
      }
    )
  },
  methods:{
    cerrar(){
      this.error=false;
      this.vacio=false;
      this.claseerror="form-control border";
      this.categoria="";
      this.fclose();
    },
    comprobar(){
      this.error=false;
      this.vacio=false;
      if(this.categoria.trim()==""){
        this.vacio=true;
        this.claseerror="form-control border border-danger";
      }
      else if(this.grupocategorias.includes(this.categoria.trim().toUpperCase())){
        this.error=true;
        this.claseerror="form-control border border-danger";
      }
      else{
        this.agregarCategoria(this.categoria.trim());
        this.cerrar();
      }
    }
  }
}
</script>

<template>
  <Transition name="fade">
    <div className="modal" v-if="visible" id="categoriaModal" tabIndex="-1" role="dialog" aria-labelledby="categoriaModalLabel" aria-hidden="true"  @click="cerrar()">
      <div className="modal-dialog" role="document"  @click="(e)=> e.stopPropagation()">
        <div className="modal-content">
          <div className="modal-header">
            <h3 className="modal-title" id="categoriaModalLabel">Crear categoria</h3>
            <button type="button" className="close" data-dismiss="modal" id="CloseModal" aria-label="Close" @click="cerrar()" >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div className="modal-body">
          <div className="row" align="center">
            <div className="col-12">					
              <form className="align-items-center pt-1 g-3 px-3">
                <div className="form-group" id="group_categoria">
                  <label htmlFor="categoriaLabel" className="form-label">Categoria</label>
                  <input type="text" :class="claseerror" v-model="categoria" id="new_categoria" name="new_categoria" alt="Nueva categoria" />
                  <Transition name="fade">
                    <p className="text-center texto_error" v-show="error">Categoria ya existente</p>
                  </Transition>
                  <Transition name="fade">
                    <p className="text-center texto_error" v-show="vacio">Rellene el campo</p>
                  </Transition>
                </div>
                <div className="form-group py-4">
                  <input type="button" className="btn btn-primary" id="crearCategoria" @click="comprobar()" value="Crear categoria" />
                </div>
              </form>
            </div>
          </div>
        </div>
        <div className="modal-footer">
          <button className="button" @click="cerrar()" >Cerrar</button>
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