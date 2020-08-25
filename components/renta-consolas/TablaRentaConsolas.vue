<template lang="pug">
  div
    h1 Tabla renta de consolas
    v-data-table(
      :items="rentals"
    )
</template>

<script>
export default {
  // props: ['rentals']
  data() {
    return {
      rentals: []
    }
  },
  created() {
    this.fillRentals()
  },
  methods: {
    async fillRentals() {
      this.rentals = await this.$axios.$get('/consolerentals')
    }
  }
}
</script>