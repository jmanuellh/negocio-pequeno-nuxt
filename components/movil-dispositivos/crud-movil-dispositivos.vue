<template lang="pug">
  div
    v-data-table(
      :headers = "headers",
      :items = "dispositivos"
    )
</template>

<script>
export default {
  data: () => ({
    headers: [
      {text: 'Nombre', value: 'nombre'},
      {text: 'MAC', value: 'mac'}
    ],
    dispositivos: []
  }),
  created() {
    this.llenarDispositivos()
  },
  methods: {
    async llenarDispositivos() {
      this.dispositivos = await this.$axios.$get('/movildispositivos')
    }
  }
}
</script>