<template>

  <section class="src-componentes-usuarios">
    <div class="jumbotron">
      <h2>Usuarios</h2>
      <hr>
      <hr>
      <br>

      <!-- <pre>{{ posts }}</pre> -->
      <!-- <pre>{{ usuarios }}</pre> -->

      <button class="btn btn-success mr-2 mb-3" @click="getUsuariosAxiosAsyncAwait()">GET ASYNC/AWAIT</button>
      <button class="btn btn-success mr-2 mb-3" @click="getUsuariosAxiosThenCatch()">GET THEN/CATCH</button>
      

      <div v-if="usuarios.length">
        <div class="table-responsive">
            <table class="table table-dark">
                <tr>
                    <th>ID</th>
                    <th>Nombre</th>
                    <th>Edad</th>
                    <th>Email</th>
                </tr>
                <tr v-for="(usuario, index) in usuarios" :key="index">
                    <td>{{ usuario.id }}</td>
                    <td>{{ usuario.nombre }}</td>
                    <td>{{ usuario.edad }}</td>
                    <td>{{ usuario.email }}</td>
                </tr>
            </table>
        </div>

        <br>
      <button class="btn btn-success mr-2 mb-3" @click="limpiarDatos()">Borrar tabla</button>
    </div>
    <h3 v-if="usuarios.length == 0 && consulta" class="alert alert-warning">
        No se encontraron usuarios.
    </h3>


    </div>
  </section>

</template>

<script>

  export default  {
    name: 'src-componentes-usuarios',
    props: [],
    mounted () {

    },
    data () {
      return {
        url: 'https://63681115d1d09a8fa6202fd8.mockapi.io/Usuarios',
        consulta: false,
        usuarios: []
      }
    },
    methods: {
      async getUsuariosAxiosAsyncAwait() {
        console.log('GET con Axios ASYNC/AWAIT ! ')
        try {
          let response = await this.axios(this.url)
          let respuesta = response.data
          this.usuarios = respuesta
        }
        catch(error) {
           console.error('ERROR en getUsuarios con Axios', error)
           this.usuarios = []
        }
        finally{
          this.consulta = true
        }
      }, 
      getUsuariosAxiosThenCatch() {
        console.log('GET con Axios THEN/CATCH ! ')
        this.axios(this.url)
        .then( respuesta => { 
          this.consulta = true 
          this.usuarios = respuesta.data
        }).catch(error =>  { 
          console.error(error)
          this.usuarios = [] 
        })
      },
      limpiarDatos() {
        this.usuarios = [],
        this.consulta = false
      },
    },
    computed: {

    }
}


</script>

<style scoped lang="css">

</style>
