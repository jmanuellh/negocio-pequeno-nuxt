<template lang="pug">
  div
    v-dialog( v-model="modalDispositivo" )
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
    limpiarNuevoDispositivo() {
      this.nuevoDispositivo = {
        nombre: "",
        mac: ""
      }
    },
    async agregarDispositivo() {
      this.modalDispositivo = false
      this.$axios.post('/movildispositivos', this.nuevoDispositivo).then(() => {
        this.llenarDispositivos()
      })
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