<template lang="pug">
  v-dialog( v-model="modalAddConsole" max-width="40rem")
    template(v-slot:activator="{on, attrs}")
      v-btn(v-bind="attrs" v-on="on" class="mx-6")
        v-icon(color="blue darken-2") mdi-plus
    v-card
      v-card-title(class="headline") Agregar Consola
      v-card-text
        v-container
          v-row
            v-col
              v-form(ref="formNewConsole")
                v-text-field(
                  v-model="newConsole.name"
                  label="Nombre"
                )
      v-card-actions
        v-spacer
        v-btn(@click="modalAddConsole = false") Cerrar
        v-btn(@click="addConsole()") Agregar
</template>

<script>
export default {
  props: {consoles: Array},
  data() {
    return {
      modalAddConsole: false,
      newConsole: {
        name: ""
      }
    }
  },
  created() {
  },
  methods: {
    addConsole() {
      this.$axios.$post('/consoles', this.newConsole).then(() => {
        this.$emit('updatedConsoles')
        this.closeModalAddConsole()
        this.resetNewConsole()
      })
    },
    closeModalAddConsole() {
      this.modalAddConsole = false
    },
    resetNewConsole() {
      this.$refs.formNewConsole.reset()
    },
    async fillConsoles() {
      this.consoles = await this.$axios.$get('/consoles')
    }
  }
}
</script>