<template>

  <section class="src-componentes-formulario">
    <div class="jumbotron">
      <h2>Formulario de Ingreso de usuarios</h2>
      <hr>
      <hr>
      <br>

      <vue-form :state="formState" @submit.prevent="enviar()">
    
        <!-- ------------------------------------------------------- -->
        <!-- Campo nombre -->
        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input 
            id="nombre"
            class="form-control"
            type="text" 
            v-model.trim="formData.nombre" 
            required 
            :minlength="nombreMinLength"
            :maxlength="nombreMaxLength"
            name="nombre"
            autocomplete="off"
          />
          <!-- cartel de validación -->
          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
            <div slot="minlength" class="alert alert-danger mt-1">Este campo debe poseer al menos {{nombreMinLength}} caracteres.</div>
            <div slot="maxLength" class="alert alert-danger mt-1">Este campo debe poseer hasta {{nombreMaxLength}} caracteres.</div>
          </field-messages>
        </validate>
        <br>

        <!-- ------------------------------------------------------- -->
        <!-- Campo edad -->
        <validate tag="div">
          <label for="edad">Edad</label>
          <input 
            id="edad"
            class="form-control"
            type="number" 
            v-model.number="formData.edad" 
            required 
            name="edad"
            autocomplete="off"
            :min="edadMin"
            :max="edadMax"
          />
          <!-- cartel de validación -->
          <field-messages name="edad" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
            <div slot="min" class="alert alert-danger mt-1">
              La edad mínima debe ser {{edadMin}} años.
            </div>
            <div slot="max" class="alert alert-danger mt-1">
              La edad máxima debe ser {{edadMax}} años.
            </div>
          </field-messages>
        </validate>
        <br>
    
        <!-- ------------------------------------------------------- -->
        <!-- Campo email -->
        <validate tag="div">
          <label for="email">Email</label>
          <input 
            id="email"
            class="form-control"
            type="email" 
            v-model="formData.email" 
            required 
            name="email"
            autocomplete="off"
          />
          <!-- cartel de validación -->
          <field-messages name="email" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
            <div slot="email" class="alert alert-danger mt-1">Debe ingresar un email válido</div>
          </field-messages>
        </validate>
        <br>

        <!-- ------------------------------------------------------- -->
        <!-- Botón de envío -->
        <button class="btn btn-success my-3" :disabled="formState.$invalid">Enviar</button>
      </vue-form>

      <hr>

    </div>

    <div v-if="usuarios.length">
        <div class="table-responsive">
            <table class="table table-dark">
                <tr>
                    <th>#</th>
                    <th>Nombre</th>
                    <th>Edad</th>
                    <th>Email</th>
                </tr>
                <tr v-for="(usuario, index) in usuarios" :key="index">
                    <td>{{ index + 1 }}</td>
                    <td>{{ usuario.nombre }}</td>
                    <td>{{ usuario.edad }}</td>
                    <td>{{ usuario.email }}</td>
                </tr>
            </table>
        </div>

        <br>
    </div>
    <h3 v-else class="alert alert-warning">
        Por el momento no se han ingresado usuarios.
    </h3>
  </section>

</template>

<script>

  export default  {
    name: 'src-componentes-formulario',
    props: [],
    mounted () {
      this.getUsuarios()
    },
    data () {
      return {
        formState : {},
        formData: this.getInitialData(),
        nombreMinLength: 3,
        nombreMaxLength: 15,
        edadMin: 18,
        edadMax: 120,
        usuarios: [],
        url: 'https://63681115d1d09a8fa6202fd8.mockapi.io/Usuarios',
      }
    },
    methods: {
      getInitialData() {
        return {
          nombre: null,
          edad: null,
          email: null
        }
      },
      async enviar() {
        let { data : usuario } = await this.axios.post(this.url, this.formData, { 'content-type' : 'application/json' })
        this.formData = this.getInitialData()
        this.formState._reset()
        this.usuarios.push(usuario)
      },
      getUsuarios() {
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
