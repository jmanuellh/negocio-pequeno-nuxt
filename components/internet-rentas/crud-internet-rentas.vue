<template lang="pug">
  div
    v-dialog( v-model="modalNuevaRenta" )
      template(v-slot:activator = "{on, attrs}")
        v-btn(
          v-on="on"
          v-bind="attrs"
        ) Agregar
      v-card
        v-card-title Agregar
        v-card-text
          v-container
            v-row
              v-col
                v-text-field(label = "Consola")
        v-card-actions 
          v-spacer
          v-btn( @click="modalNuevaRenta = false" ) Cerrar
          v-btn( @click="" ) Agregar
    v-data-table(
      :headers = "headers",
      :items = "rentas"
    )
      
</template>

<script>
export default {
  data: () => ({
    modalNuevaRenta: false,
    headers: [
      {text: 'Dispositivo', value: 'movilDispositivo.nombre'},
      {text: 'Fecha Fin', value: 'fechaFin'},
      {text: 'Dinero', value: 'dinero'}
    ],
    rentas: []
  }),
  created() {
    this.llenarRentas()
  },
  methods: {
    async llenarRentas() {
      this.rentas = await this.$axios.$get('/internetrentas')
    }
  }
}
</script>