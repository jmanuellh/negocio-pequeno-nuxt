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
                  modal-add-console(:consoles="consoles" @updatedConsoles="fillConsoles()")
                v-col(cols="12" justify-center )
                  v-time-picker(v-model="dates.startDate")
                  v-time-picker(v-model="dates.endDate")
                v-col(cols="6")
                  v-text-field(v-model="dates.startDate" @change="cambioFechaInicio" label="Hora incio" type="time")
                v-col(cols="6")
                  v-text-field(v-model="dates.endDate" label="Hora fin" type="time")
                    
          v-card-actions
            v-spacer
            v-btn(@click="dialogModificarRenta = false") Cerrar
            v-btn( @click="agregarRenta()") Agregar
            
      //- v-btn(
      //-   @click="agregarRenta()"
      //-   class="mx-5" 
      //- ) Agregar
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
    },
    showModalAddConsole() {
      this.modalAddConsole = true
    },
    async fillConsoles() {
      const consoles = await this.$axios.$get('/consoles')
      console.log('consoles: ', consoles)
      this.consoles = consoles
    },
    cerrarModalAgregarRenta() {
      this.limpiarModalModificarRenta()
      this.modalModificarRenta = false
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
      this.$axios.$post('/consoleRentals', this.nuevaRenta).then(() => {
        this.$refs.tablaRentaConsolas.fillRentals()
      })
      this.modalModificarRenta = false
      this.limpiarModalModificarRenta()
    }
  }
}
</script>
