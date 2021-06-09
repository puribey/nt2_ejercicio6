<template>
  <section class="mt-3">
    <div class="container-fluid">
      <h2>Usuarios</h2>
      <hr />

      <div v-if="loading" class="alert alert-warning mb-3">
        Loading usuarios
      </div>
      <div v-if="!loading">
        <div v-if="users.length" class="alert alert-primary mb-3">
          Se encontraron {{ users.length }} usuarios.
        </div>
        <div v-else class="alert alert-secondary mb-3">
          No se encontraron usuarios
        </div>
        <div v-if="users.length" class="table-container mt-3">
          <div class="table-responsive">
            <table class="table table-striped table-bordered mt-3">
              <!-- encabezado de la tabla -->
              <thead>
                <tr>
                  <th v-for="(col, index) in getCols" :key="index">
                    {{ col }}
                  </th>
                </tr>
              </thead>

              <!-- filas con los datos -->
              <tbody>
                <tr v-for="(user, index) in users" :key="index">
                  <td v-for="(col, index) in getCols" :key="index">
                    {{ user[col] }}
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script lang="js">

  export default  {
    name: 'ListaUsuarios',
    props: [],
    mounted() {
      this.loading = true
      this.getUsers()
    },
    data () {
      return {
        url : 'https://60a5969fc0c1fd00175f40c0.mockapi.io/api/users',
        users : [],
        loading: false
      }
    },
    methods: {
      async getUsers() {
        try {
          let respuesta = await this.axios(this.url)
          setTimeout(() => {
            this.loading = false
          }, 1000);
          this.users = respuesta.data
        }
        catch(error) {
          console.log(error)
        }
      },
    },
    computed: {
      getCols() {
        return Object.keys(this.users[0])
      }
    }
}


</script>

<style scoped lang="css">
.table-container {
  height: 500px;
  overflow: scroll;
}
.row button {
  width: 100%;
}
</style>
