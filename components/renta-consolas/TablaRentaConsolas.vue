<template lang="pug">
  div
    v-data-table(    
      :headers="headers"
      :items="rentals"
    )
</template>

<script>
export default {
  // props: ['rentals'],
  data() {
    return {
      headers:[
        {text: 'Consola', value: 'consoleId'}
      ],
      rentals: [{id: 1}]
    }
  },
  created() {
    this.fillRentals()
  },
  methods: {
    async fillRentals() {
      const rentals = await this.$axios.$get('/consolerentals')
      console.log('rentals: ', rentals)
      this.rentals = rentals
    }
  }
}
</script>