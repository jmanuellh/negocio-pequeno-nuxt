<template lang="pug">
  div
    v-dialog( v-model="modalEliminarConsola" max-width="40rem")
      template( v-slot:activator="{ on }")
        v-btn(
          v-on="on"
        )
          v-icon mdi-delete
          span Eliminar Consola
      v-card()
        v-card-title ¿Eliminar consola?
        v-card-text ¿Eliminar consola?
        v-card-actions
          v-spacer
          v-btn( @click="modalEliminarConsola = false" ) Cancelar
          v-btn(@click="eliminarConsola()") Eliminar
            
</template>

<script>
export default {
  props: ['signal'],
  data() {
    return {
      modalEliminarConsola: false
    }
  },
  methods: {
    eliminarConsola() {
      this.modalEliminarConsola = false
      this.$axios.delete(`/consoles/${this.signal}`).then(() => {
        this.updatedConsoles()
      })
    },
    updatedConsoles() {
      this.$emit('updatedConsoles')
    }
  }
}
</script>

