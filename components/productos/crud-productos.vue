<template lang="pug">
  div
    div( class="d-flex justify-between" )
      //- Buscar
      v-text-field(
        class="my-5"
        class="ml-5"
        label="Buscar producto"
        v-model="search.nombre"
        @keyup.enter="searchProduct"
      )
      v-btn(
        @click="searchProduct"
        class="my-5 mr-5"
      ) Buscar
        v-icon(class="ml-2" ) mdi-magnify

      //- Agregar
      v-dialog(
        v-model="dialogNewProduct"
        @input="showDialogNewProducto"
      )
        template( v-slot:activator = "{ on, attrs }" )
          v-btn(
            v-on="on"
            v-bind="attrs"
            class="ma-5"
            color="primary"
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
              span(v-if="newProduct.id == undefined")
                v-btn( @click="addProduct" :disabled="!enabledBtnAddProduct") Agregar
              span(v-else)
                v-btn( @click="updateProduct" :disabled="!enabledBtnAddProduct") Actualizar
    v-data-table(
      style="align-text=right"
      :headers = "headers",
      :items = "productos"
      :loading = "loadingProduct"
      loading-text="Cargando"
    )
      template( v-slot:item.nombre="{ item }" )
        span(class="text-h5") {{ item.nombre }}
      template( v-slot:item.precioVenta="{ item }" )
        span(class="text-h5") {{ item.precioVenta }}
      template( v-slot:item.acciones = "{ item }" class="d-flex" )
            v-btn( @click="editingProduct(item)" ) Editar
            v-dialog(
              v-model="dialogDeleteProduct"
              :retain-focus="false"
              @input="showDialogDeleteProduct"
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
                  v-btn( @click="dialogDeleteProduct = false" ) Cerrar
                  v-btn(
                    @click="deleteProduct(item.id)"
                    :disabled="!isEnabledBtnDeleteProduct"
                  ) Eliminar
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
      search: {
        nombre: null
      },
      loadingProduct: true,
      productos: [],
      headers:[
        {text: 'Nombre', value: 'nombre',},
        {text: 'Precio', value: 'precioVenta',},
        {text: 'Acciones', value: 'acciones', sortable:false, align: 'center'}
      ],
      mensajeCamposVacios: "Debe llenar los campos",
      enabledBtnAddProduct: true,
      dialogNewProduct: false,
      dialogDeleteProduct: false,
      newProduct: {
        nombre: "",
        precioVenta: null
      },
      snackbar: false,
      isEnabledBtnDeleteProduct: true
    }
  },
  mounted() {
    this.getProducts()
    this.getProductsPaginated()
  },
  methods: {
    showDialogNewProducto(show) {
      if(!show) this.cleanNewProduct()
      else this.enabledBtnAddProduct = true
    },
    showDialogDeleteProduct(show) {
      if (show) this.isEnabledBtnDeleteProduct = true
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
    updateProduct() {
      this.enabledBtnAddProduct = false
      this.$axios.put("/product/"+this.newProduct.id, this.newProduct).then(() => {
        this.getProducts()
        this.dialogNewProduct = false
        this.cleanNewProduct()
      })
    },
    async getProducts() {
      this.loadingProduct = true
      try {
        this.productos = await this.$axios.$get("/product")
      } finally {
        this.loadingProduct = false
      }
    },
    deleteProduct(id) {
      this.isEnabledBtnDeleteProduct = false
      this.$axios.delete("/product/"+id).then(() => {
        this.getProducts()
        this.dialogDeleteProduct = false
      })
    },
    editingProduct(producto) {
      this.newProduct = producto
      this.enabledBtnAddProduct = true
      this.dialogNewProduct = true
    },
    searchProduct() {
      this.loadingProduct = true
      this.$axios.post("/product/search", {nombre: this.search.nombre}).then(response => {
        this.productos = response.data
        this.loadingProduct = false
      })
    },
    cleanSearch() {
      this.search = {
        nombre: null
      }
    },
    getProductsPaginated() {
      this.$axios.post("/product/paginated", {pageNumber: 2, pageSize: 1}).then(r => {
        console.log(r.data)
      }).catch(e => console.log(e))
    }
  }
}
</script>

<style lang="scss" scoped>



</style>