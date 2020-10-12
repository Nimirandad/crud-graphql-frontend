<template>
  <div class="container">
    <form action="" @submit.prevent="mounted()">
      <div class="input-group mb-3 buscador">
        <input v-model="empleado" empleado="" type="text" class="form-control" placeholder="Nombre Empleado" aria-label="Nombre Empleado" aria-describedby="button-addon2"/>
        <div class="input-group-append">
          <button class="btn btn-outline-secondary" type="submit" id="button-addon2">Buscar</button>
        </div>
      </div>
    </form>
    <div class="col-sm-12 container tabla">
      <table class="table table-striped mt-4">
        <thead>
          <tr>
            <th scope="col">Nombre Empleado</th>
            <th scope="col">Titulo</th>
            <th scope="col">Ciudad</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="input in directorio.edges" :key="input.id">
            <td>{{ input.node.nombreEmpleado }}</td>
            <td>{{ input.node.tituloEmpleado.nombreTitulo }}</td>
            <td>{{ input.node.ciudadEmpleado.nombreCiudad }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'CompaniaData',
  data () {
    return {
      empleado: '',
      directorio: []
    }
  },
  methods: {
    async mounted () {
      try {
        const result = await axios({
          method: 'POST',
          url: 'http://127.0.0.1:8000/graphql/',
          data: {
            query:
              `
            {
              allEmpleados(nombreEmpleado: "` + this.empleado + `"){
                edges{
                  node{
                    id
                    nombreEmpleado
                    tituloEmpleado {
                      nombreTitulo
                    }
                    ciudadEmpleado {
                      nombreCiudad
                    }
                  }
                }
              }
            }
          `
          }
        })
        this.directorio = result.data.data.allEmpleados
      } catch (error) {
        console.log(error.message)
      }
    }
  }
}
</script>

<style scoped>
.buscador {
  margin-top: 20px;
  align-content: right;
}

.tabla {
  margin-top: 50px;
}
</style>
