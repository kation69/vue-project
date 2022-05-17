<script>
export default{
  props:{
    visible: {
      type: Boolean,
      required: true
    },
    fclose:{
      type:Function
    },
    flogados:{
      type:Function
    }
  },
  data(){
    return(
      {
        nombre:"",
        contrasenia:"",
        error:false
      }
    )
  },
  methods:{
    cerrar(){
      this.error=false;
      this.nombre="";
      this.contrasenia="";
      this.fclose();
    },
    comprobar(){
      if(this.nombre=="admin" && this.contrasenia=="admin"){
        this.error=false;
        this.flogados();
        this.fclose();
      }
      else{
        this.error=true;
      }
    }
  }
}
</script>


<template>
  <Transition name="fade">
    <div className="modal" v-if="this.visible" tabIndex="-1" role="dialog" aria-labelledby="categoriaModalLabel" aria-hidden="true" @click="cerrar()" >
    <div className="modal-dialog" role="document" @click="(e)=> (e.stopPropagation())">
      <div className="modal-content">
        <div className="modal-header">
          <h3 className="modal-title" id="loginModalLabel">Inicia sesión</h3>
          <button type="button" className="close" data-dismiss="modal" id="CloseModal" aria-label="Close" @click="cerrar()">
              <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <div className="modal-body">

          <div className="row" align="center">
            <div className="col-12">					
              <form className="align-items-center pt-1 g-3 px-3">
                <div className="form-group">
                  <label htmlFor="usuarioLabel" className="form-label">Usuario</label>
                  <input type="text" className="form-control border" id="usuario" name="usuario" alt="Usuario" placeholder="Escribe usuario" v-model="nombre"/>
                </div>
                <div className="form-group pt-1">
                  <label htmlFor="passLabel" className="form-label">Contraseña</label>
                  <input className="form-control border" type="password" id="contrasenia" name="password"  placeholder="Escribe contraseña"  v-model="contrasenia"/>
                </div>
                <div className="form-group py-4">
                  <Transition name="fade">
                    <p className="text-danger" v-show="error">Error de usuario o contraseña</p>
                  </Transition>
                  <input type="button" className="btn btn-primary" id="acceder" value="Acceder" @click="comprobar()"/>
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