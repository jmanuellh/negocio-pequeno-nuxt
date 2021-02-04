<template lang="pug">
  div
    v-dialog( v-model="dialogNewProduct" @input="showDialogNewProducto" )
      template( v-slot:activator = "{ on, attrs }" )
        v-btn(
          v-on="on"
          v-bind="attrs"
        ) Agregar
      v-card
        v-card-title Agregar Producto
        v-card-text
          v-container
            v-row
              v-col
                v-text-field(
                  v-model="newProduct.nombre"
                  label="Nombre"
                )
              v-col
                v-text-field(
                  type="number" 
                  v-model.number="newProduct.precioVenta"
                  label="Precio"
                )
          v-card-actions
            v-btn( @click="closeDialogNewProduct" ) Cerrar
            v-btn( @click="addProduct" ) Agregar producto
</template>

<script>
export default {
  data() {
    return {
      dialogNewProduct: false,
      newProduct: {
        nombre: "",
        precioVenta: null
      }
    }
  },
  methods: {
    showDialogNewProducto(show) {
      if(!show) this.cleanNewProduct()

      // // Esto es para que al salir de editar, no se quede guardado el id al crear un nuevo producto
      // if (show && this.newProduct.id != undefined) {
      //   this.newProduct.id = undefined
      // }
    },
    closeDialogNewProduct() {
      this.cleanNewProduct()
      this.dialogNewProduct = false
    },
    cleanNewProduct() {
      this.newProduct = {
        nombre: "",
        precioVenta: null
      }
    },
    addProduct() {
    }
  }
}
</script>