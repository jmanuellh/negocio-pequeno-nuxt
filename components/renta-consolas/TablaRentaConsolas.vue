<template lang="pug">
  div
    v-data-table(    
      :headers="headers"
      :items="rentals"
    )
      template(v-slot:item.pagado="{ item }")
        v-simple-checkbox(v-model="item.pagado")
      template(v-slot:item.startDate="{ item }") {{$moment.utc(item.startDate).local().format('h:mm a')}}
      template(v-slot:item.endDate="{ item }") {{$moment.utc(item.endDate).local().format('h:mm a')}}
</template>

<script>
export default {
  // props: ['rentals'],
  data() {
    return {
      saludo: "Hola",
      headers:[
        {text: 'Consola', value: 'console.name'},
        {text: 'Persona', value: 'customerName'},
        {text: 'Inicio', value: 'startDate'},
        {text: 'Fin', value: 'endDate'},
        {text: 'Pagado', value: 'pagado'}
      ],
      rentals: []
    }
  },
  created() {
    this.fillRentals()
    this.saludo = this.$moment()
  },
  methods: {
    async fillRentals() {
      const rentals = await this.$axios.$get('/consolerentals')
      console.log('rentals: ', rentals)
      this.rentals = rentals
      // console.log(this.$moment(rentals[0].startDate).tz(Intl.DateTimeFormat().resolvedOptions().timeZone))
    }
  }
}
</script>