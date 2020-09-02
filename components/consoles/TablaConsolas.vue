<template lang="pug">
  div
    v-data-table(    
      :headers="headers"
      :items="consoles"
    )
</template>

<script>
export default {
  data() {
    return {
      headers:[
        {text: 'Consola', value: 'name'},
      ],
      consoles: []
    }
  },
  created() {
    this.fillConsoles()
  },
  methods: {
    async fillConsoles() {
      const consoles = await this.$axios.$get('/consoles')
      this.consoles = consoles
    }
  }
}
</script>