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
      template(v-slot:item.acciones="{ item }")
        v-dialog( v-model="modalEliminarRentaConsola" max-width="40rem" :retain-focus="false")
          template( v-slot:activator="{ on, attrs }")
            v-btn(
              v-on="on"
              v-bind="attrs"
            )
              v-icon mdi-delete
              span Eliminar Renta de consola
          v-card()
            v-card-title ¿Eliminar Renta de consola?
            v-card-text ¿Eliminar Renta de consola?
            v-card-actions
              v-spacer
              v-btn( @click="modalEliminarRentaConsola = false" ) Cancelar
              v-btn(@click="eliminarRentaConsola(item.id)") Eliminar
</template>

<script>
export default {
  data() {
    return {
      modalEliminarRentaConsola: false,
      headers:[
        {text: 'Consola', value: 'console.name'},
        {text: 'Persona', value: 'customerName'},
        {text: 'Inicio', value: 'startDate'},
        {text: 'Fin', value: 'endDate'},
        {text: 'Pagado', value: 'pagado'},
        {text: 'Acciones', value: 'acciones'}
      ],
      rentals: []
    }
  },
  created() {
    this.fillRentals()
    this.saludo = this.$moment()
  },
  methods: {
    async eliminarRentaConsola(id) {
      this.modalEliminarRentaConsola = false
      this.$axios.delete(`/consolerentals/${id}`).then(() => {
        this.fillRentals()
      })
    },
    async fillRentals() {
      const rentals = await this.$axios.$get('/consolerentals')
      this.rentals = rentals
    }
  }
}
</script>