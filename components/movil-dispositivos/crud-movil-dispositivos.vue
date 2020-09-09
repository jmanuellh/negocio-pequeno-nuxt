<template lang="pug">
  div
    v-dialog( v-model="modalDispositivo" @input="mostrarModalDispositivo" )
      template( v-slot:activator = "{ on, attrs }" )
        v-btn(
          v-on = "on",
          v-bind = "attrs"
        ) Agregar
      v-card(  )
        v-card-title Agregar
        v-card-text
          v-container
            v-row
              v-col
                v-text-field( v-model="nuevoDispositivo.nombre"  label = "Nombre" )
              v-col
                v-text-field( v-model="nuevoDispositivo.mac"  label = "MAC" )
        v-card-actions
          v-btn( @click="modalDispositivo = false" ) Cerrar
          v-btn( @click="agregarDispositivo()" ) Agregar
    v-data-table(
      :headers = "headers",
      :items = "dispositivos"
    )
      template(v-slot:item.acciones="{ item }")
        v-btn( @click="editarDispositivo(item)" ) Editar
        v-dialog(
          v-model="modalEliminarDispositivo"
          :retain-focus="false"
        )
          template( v-slot:activator = "{ on, attrs }" )
            v-btn(
              v-on = "on"
              v-bind = "attrs"
            ) Eliminar
          v-card
            v-card-title Eliminar
            v-card-actions
              v-spacer
              v-btn( @click="modalEliminarDispositivo = false" ) Cerrar
              v-btn( @click="eliminarDispositivo(item.id)" ) Eliminar
</template>

<script>
export default {
  data: () => ({
    modalEliminarDispositivo: false,
    modalDispositivo: false,
    nuevoDispositivo: {
      nombre: "",
      mac: ""
    },
    headers: [
      {text: 'Nombre', value: 'nombre'},
      {text: 'MAC', value: 'mac'},
      {text: 'Acciones', value: 'acciones'}
    ],
    dispositivos: []
  }),
  created() {
    this.llenarDispositivos()
  },
  methods: {
    mostrarModalDispositivo(mostrar) {
      // Esto es para que al salir de editar no se quede guardado el id al crear una nueva
      if (mostrar && this.nuevoDispositivo.id != undefined) {
        this.nuevoDispositivo.id = undefined
      }
    },
    async editarDispositivo(item) {
      this.nuevoDispositivo = Object.assign({}, item)
      // this.nuevoDispositivo = Object.assign({}, this.dispositivos.find(dispositivo => dispositivo.id == id))
      // this.nuevoDispositivo = await this.$axios.$get(`/movilDispositivos/${id}`)
      this.modalDispositivo = true
    },
    limpiarNuevoDispositivo() {
      this.nuevoDispositivo = {
        nombre: "",
        mac: ""
      }
    },
    async agregarDispositivo() {
      this.modalDispositivo = false
      if (this.nuevoDispositivo.id == undefined) {
        this.$axios.post('/movildispositivos', this.nuevoDispositivo).then(() => {
          this.llenarDispositivos()
        })        
      } else {
        this.$axios.put(`/movildispositivos/${this.nuevoDispositivo.id}`, this.nuevoDispositivo).then(() => {
          this.llenarDispositivos()
        })
      }
      this.limpiarNuevoDispositivo()
    },
    async llenarDispositivos() {
      this.dispositivos = await this.$axios.$get('/movildispositivos')
    },
    async eliminarDispositivo(id) {
      this.modalEliminarDispositivo = false
      this.$axios.delete(`/movildispositivos/${id}`).then(() => {
        this.llenarDispositivos()
        this.limpiarNuevoDispositivo()
      })
    }
  }
}
</script>