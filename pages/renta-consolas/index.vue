<template lang="pug">
  div
    h1 Renta de Consolas
    //- TablaRentaConsolas(:rentals="rentals")
    crud-renta-consolas
</template>

<script>
// import https from 'https'

export default {
  // async asyncData(context) {
  //   // context.app.$axios.defaults.httpsAgent = new https.Agent({ rejectUnauthorized: false })
  //   return await context.app.$axios.get(
  //     '/consolerentals',
  //     {
  //       httpsAgent: new https.Agent({
  //         rejectUnauthorized: false
  //       })
  //     }
  //     ).then(r=> {
  //       console.log(r.data)
  //       return {
  //         rentals: r.data
  //       }
  //     })
  // }
}
</script>
