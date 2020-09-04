<template lang="pug">
  div
    template(v-slot:item.acciones="{ item }")
      v-dialog( v-model="modalEliminarConsola" max-width="40rem")
        template( v-slot:activator="{ on }")
          v-btn(
            v-on="on"
          )
            v-icon mdi-delete
            span Eliminar Renta de consola
        v-card()
          v-card-title ¿Eliminar Renta de consola?
          v-card-text ¿Eliminar Renta de consola?
          v-card-actions
            v-spacer
            v-btn( @click="modalEliminarConsola = false" ) Cancelar
          v-btn(@click="eliminarRentaConsola(item.id)") Eliminar
</template>

<script>
export default {
  data() {
    modalEliminarRentaConsola = false
  },
  methods: {
    eliminarRentaConsola(id) {
      modalEliminarRentaConsola = false
      this.$axios.delete(`/consolerentals/${id}`).then(()=> {
        this.updatedConsoleRentals()
      })
    },
    updatedConsoleRentals() {
      this.$emit('updatedConsoleRentals')
    }
  }
}
</script>