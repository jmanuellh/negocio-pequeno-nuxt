<template lang="pug">
  div
    div( class="d-flex justify-end" )
      v-dialog(
        v-model="dialogNewProduct"
        @input="showDialogNewProducto"
      )
        template( v-slot:activator = "{ on, attrs }" )
          v-btn(
            v-on="on"
            v-bind="attrs"
            class="ma-5"
          ) Agregar
        v-card
          v-card-title Agregar Producto
          v-card-text
            v-container
              v-row
                v-col(cols="12" lg="6")
                  v-text-field(
                    v-model="newProduct.nombre"
                    label="Nombre"
                  )
                v-col(cols="12" lg="6")
                  v-text-field(
                    type="number" 
                    v-model.number="newProduct.precioVenta"
                    label="Precio"
                  )
            v-card-actions(class="d-flex justify-end" )
              v-btn( @click="closeDialogNewProduct" ) Cerrar
              v-btn( @click="addProduct" :disabled="!enabledBtnAddProduct") Agregar
    v-snackbar(
      v-model="snackbar"
    ) {{ mensajeCamposVacios }}
      template(v-slot:action="{ attrs }")
        v-btn(
          text
          v-bind="attrs"
          @click="snackbar = false") Cerrar
</template>

<script>
export default {
  data() {
    return {
      mensajeCamposVacios: "Debe llenar los campos",
      enabledBtnAddProduct: true,
      dialogNewProduct: false,
      newProduct: {
        nombre: "",
        precioVenta: null
      },
      snackbar: false
    }
  },
  methods: {
    showDialogNewProducto(show) {
      if(!show) this.cleanNewProduct()
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
      if(this.newProduct == "" || this.newProduct.precioVenta == null) {
        this.snackbar = true
      } else {
        this.enabledBtnAddProduct = false
        this.$axios.post(
          "/product",
          this.newProduct)
          .then(() => {
            this.getProducts()
            this.cleanNewProduct()
            this.dialogNewProduct = false
            this.enabledBtnAddProduct = true
          })

      }
    },
    getProducts() {

    }
  }
}
</script>