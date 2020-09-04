<template lang="pug">
  div
    v-data-table(    
      :headers="headers"
      :items="consoles"
    )
      template(v-slot:item.acciones="{ item }")
        slot(:signal="item.id")
</template>

<script>
export default {
  props: {
    btnEliminar: {
      type: Boolean,
      default: true
    },
    btnEditar: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      headers: [],
      consoles: []
    }
  },
  created() {
    this.fillConsoles()
    if(this.btnEliminar || this.btnEditar) {
      this.headers = [
        {text: 'Consola', value: 'name'},
        {text: 'Acciones', value: 'acciones', sortable: false}
      ]
    } else {
      this.headers = [
        {text: 'Consola', value: 'name'},
      ]
    }
  },
  methods: {
    async fillConsoles() {
      const consoles = await this.$axios.$get('/consoles')
      this.consoles = consoles
    }
  }
}
</script>