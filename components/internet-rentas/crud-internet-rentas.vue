<template lang="pug">
  div
    v-dialog( v-model="modalNuevaRenta" )
      template(v-slot:activator = "{on, attrs}")
        v-btn(
          v-on="on"
          v-bind="attrs"
          @click="mostrarModalNuevaRenta()"
        ) Agregar
      v-card
        v-card-title Agregar
        v-card-text
          v-container
            v-row
              v-col(cols="12" sm="12")
                v-select(
                  label="Dispositivo"
                  :items="dispositivos"
                  item-text="nombre"
                  item-value="id"
                  v-model="nuevaRenta.movilDispositivoId"
                )
              v-col(cols="12" sm="6")
                v-text-field(
                  label="Fecha fin"
                  type="date"
                  @change="cambioFechaFin()"
                  v-model="fechaFin"
                )
              v-col(cols="12" sm="6")
                v-text-field(
                  label="Dinero"
                  type="number"
                  v-model.number="nuevaRenta.dinero"
                )
        v-card-actions 
          v-spacer
          v-btn( @click="modalNuevaRenta = false" ) Cerrar
          v-btn( @click="agregarRenta()" ) Agregar
    v-data-table(
      :headers = "headers",
      :items = "rentas"
    )
      template(v-slot:item.fechaFin="{item}")
        span {{$moment.utc(item.fechaFin).local().format('YYYY MMMM DD dddd')}}
      template( v-slot:item.acciones = "{ item }" class="d-flex" )
            v-btn( @click="mostrarModalNuevaRenta(item)" ) Editar
            v-dialog(
              v-model="modalEliminarRenta"
              :retain-focus="false"
            )
              template( v-slot:activator = "{ on, attrs }" )
                v-btn(
                  v-on = "on"
                  v-bind = "attrs"
                ) Eliminar
              v-card
                v-card-title Eliminar
                v-card-actions
                  v-spacer
                  v-btn( @click="modalEliminarRenta = false" ) Cerrar
                  v-btn( @click="eliminarRenta(item.id)" ) Eliminar
</template>

<script>
export default {
  data: () => ({
    modalNuevaRenta: false,
    modalEliminarRenta: false,
    headers: [
      {text: 'Dispositivo', value: 'movilDispositivo.nombre'},
      {text: 'Fecha Fin', value: 'fechaFin'},
      {text: 'Dinero', value: 'dinero'},
      {text: 'Acciones', value: 'acciones'}
    ],
    rentas: [],
    dispositivos: [],
    nuevaRenta: {
      movilDispositivoId: "",
      fechaFin: "",
      dinero: ""
    },
    fechaFin: ""
  }),
  created() {
    this.llenarRentas()
  },
  methods: {
    cerrarModalNuevaRenta() {
      this.limpiarNuevaRenta()
      this.modalNuevaRenta = false
    },
    mostrarModalNuevaRenta(item) {
      this.llenarDispositivos()
      if (item != undefined) {
        this.nuevaRenta = Object.assign({}, item)
        this.fechaFin = this.$moment(this.nuevaRenta.fechaFin).format('YYYY-MM-DD')
        console.log(this.fechaFin)
        this.modalNuevaRenta =  true
      } else {
        this.nuevaRenta.id = undefined
        this.nuevaRenta.movilDispositivo = null
      }
    },
    async eliminarRenta(id) {
      this.modalEliminarRenta = false
      this.$axios.delete(`/internetRentas/${id}`).then(() => {
        this.llenarRentas()
      })
    },
    cambioFechaFin() {
      this.nuevaRenta.fechaFin = this.$moment(this.fechaFin)
    },
    limpiarNuevaRenta() {
      this.nuevaRenta = {
        movilDispositivoId: "",
        fechaFin: "",
        dinero: ""
      }
      this.fechaFin = ""
    },
    async llenarDispositivos() {
      this.dispositivos = await this.$axios.$get('/movilDispositivos')
    },
    async llenarRentas() {
      this.rentas = await this.$axios.$get('/internetRentas')
    },
    async agregarRenta() {
      this.modalNuevaRenta = false
      if (this.nuevaRenta.id == undefined) {
        this.$axios.post('/internetrentas', this.nuevaRenta).then(() => {
          this.llenarRentas()
        })
      } else {
        this.$axios.put(`/internetrentas/${this.nuevaRenta.id}`, this.nuevaRenta).then(() => {
          this.llenarRentas()
        })
      }
      this.limpiarNuevaRenta()
    }
  }
}
</script>