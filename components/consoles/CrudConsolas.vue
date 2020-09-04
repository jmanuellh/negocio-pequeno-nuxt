<template lang="pug">
  div
    div(class="d-flex justify-end")
      modal-add-console(@updatedConsoles="$refs.tablaConsolas.fillConsoles()")
    tabla-consolas(ref="tablaConsolas" :btnEditar="false")
      template(v-slot:default="slotProps")
        btn-eliminar-consola(
          @updatedConsoles="$refs.tablaConsolas.fillConsoles()"
          :signal="slotProps.signal"
        )
</template>
