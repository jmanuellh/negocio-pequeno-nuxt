<template lang="pug">
  div
    h1 Renta de Consolas
    //- TablaRentaConsolas(:rentals="rentals")
    TablaRentaConsolas
</template>

<script>
export default {
  // async asyncData(context) {
  //   const rentals = await context.app.$axios.$get('/consolerentals')
  //   return {
  //     rentals: rentals
  //   }
  // }
}
</script>