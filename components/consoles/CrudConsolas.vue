<template lang="pug">
  div
    div(class="d-flex justify-end")
      modal-add-console(@updatedConsoles="$refs.tablaConsolas.fillConsoles()")
    tabla-consolas(ref="tablaConsolas")
</template>
