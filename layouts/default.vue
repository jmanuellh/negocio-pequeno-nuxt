<template lang="pug">
  v-app
    v-navigation-drawer(app v-model="drawer")
      v-list
        v-list-item(to="/")
          v-list-item-content
            v-list-item-title Inicio
        v-list-item(to="/renta-consolas")
          v-list-item-content
            v-list-item-title Renta de consolas
        v-list-item(to="/consolas")
          v-list-item-content
            v-list-item-title Consolas

    v-app-bar(
      app
      color="indigo"
      dark
    )
      v-app-bar-nav-icon(@click.stop="drawer = !drawer")
      v-toolbar-title Pequeño negocio
    v-main
      Nuxt
</template>

<script>
export default {
  data() {
    return {
      drawer: false
    }
  }
}
</script>

<style>
</style>
