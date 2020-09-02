<template lang="pug">
  div
    div(class="d-flex justify-end")
      v-dialog( v-model="dialogModificarRenta" max-width="40rem")
        template( v-slot:activator="{ on, attrs }" )
          v-btn(
            @click="fillConsoles()"
            v-bind="attrs"
            v-on="on"
            class="mx-6") Agregar
        v-card
          v-card-title(class="headline") Rentar consola
          v-card-text
            v-container
              v-row
                v-col(cols="8")
                  v-select(
                    :items="consoles"
                    v-model="nuevaRenta.consoleId"
                    label="Consola"
                    item-text="name"
                    item-value="id"
                  )

                v-col(cols="4" class="d-flex justify-center align-center")
                  modal-add-console(@updatedConsoles="fillConsoles()")
                    v-icon(color="blue darken-2") mdi-plus
                v-col(cols="12")
                  v-text-field(
                    label="Persona"
                    v-model="nuevaRenta.customerName"
                  )
                v-col(cols="6")
                  v-text-field(v-model="dates.startDate" @change="cambioFechaInicio()" label="Hora incio" type="time")
                v-col(cols="6")
                  v-text-field(v-model="dates.endDate" @change="cambioFechaFin()" label="Hora fin" type="time")
          v-card-actions
            v-spacer
            v-btn(@click="dialogModificarRenta = false") Cerrar
            v-btn( @click="agregarRenta()") Agregar
    TablaRentaConsolas(ref="tablaRentaConsolas")
</template>

<script>
import moment from 'moment';

export default {
  data() {
    return {
      picker: null,
      dates: {
        startDate: "",
        endDate: ""
      },
      consoles: [{id: "", name: ""}],
      nuevaRenta: {
        consoleId: "",
        startDate: "",
        endDate: "",
        customerName: ""
      },
      dialogModificarRenta: false,
      modalAddConsole: false,
    }
  },
  created() {
    console.log('moment: ',moment())
  },
  methods: {
    cambioFechaInicio() {
      const horas = this.dates.startDate.split(":")[0]
      const minutos = this.dates.startDate.split(":")[1]
      this.nuevaRenta.startDate = this.$moment().set({'h': horas, 'm': minutos})
    },
    cambioFechaFin() {
      const horas = this.dates.endDate.split(":")[0]
      const minutos = this.dates.endDate.split(":")[1]
      this.nuevaRenta.endDate = this.$moment().set({'h': horas, 'm': minutos})
    },
    showModalAddConsole() {
      this.modalAddConsole = true
    },
    async fillConsoles() {
      const consoles = await this.$axios.$get('/consoles')
      this.consoles = consoles
    },
    cerrarModalAgregarRenta() {
      this.limpiarModalModificarRenta()
      this.dialogModificarRenta = false
    },
    limpiarModalModificarRenta() {
      this.nuevaRenta = {
        consoleId: "",
        startDate: "",
        endDate: "",
        customerName: ""
      }
    },
    async agregarRenta() {
      console.log(this.nuevaRenta)
      this.$axios.post('/consoleRentals', this.nuevaRenta).then(() => {
        this.$refs.tablaRentaConsolas.fillRentals()
        this.dialogModificarRenta = false
        this.limpiarModalModificarRenta()
      })
    }
  }
}
</script>
