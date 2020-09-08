<template>
  <div class="addproduct">
    <h1>Agregar Producto</h1>
    <div class="form">
      <b-form @submit.prevent="agregarProducto(producto)">
        <label for="feedback-user">Descripción</label>
        <b-input v-model="producto.Descripcion" :state="validation" id="feedback-user"></b-input>
        <!-- <b-form-invalid-feedback :state="validation">¡La descripción es requerida!</b-form-invalid-feedback> -->
        <!-- <b-form-valid-feedback :state="validation">Looks Good.</b-form-valid-feedback> -->
        <br />
        <label for="feedback-user">Stock</label>
        <b-input v-model="producto.Stock" :state="validation" id="feedback-user"></b-input>
        <br />
        <label for="feedback-user">Precio de Venta</label>
        <b-input v-model="producto.PrecioVenta" :state="validation" id="feedback-user"></b-input>
        <br />
        <label for="feedback-user">Proveedor</label>
        <b-input v-model="producto.Proveedor" :state="validation" id="feedback-user"></b-input>
        <br />
        <label for="feedback-user">Sucursal</label>
        <b-input v-model="producto.Sucursal" :state="validation" id="feedback-user"></b-input>
        <br />
        <b-button class="btn-sm btn-block btn-success" type="submit">Agregar</b-button>
      </b-form>
    </div>

    <!-- --------------Modal ----------------------- -->
    <div>
      <b-modal ref="my-modal" hide-footer title="">
        <div class="d-block text-center">
          <h3 v-if="success">Producto creado exitosamente.</h3>
          <h3 v-if="!success">Producto no puedo ser creado.</h3>
        </div>
        <b-button class="mt-3" variant="success" block @click="hideModal">Entendido</b-button>
      </b-modal>
    </div>
  </div>
</template>

<script>
export default {
  name: "productForm",
  data() {
    return {
      producto: {},
      success: false
    };
  },
  methods: {
    agregarProducto(item) {
      this.axios
        .post("/productos", item)
        .then(res => {
          //   this.producto.unshift(res.data);
          console.log(res);
          this.success = true;
          this.showModal();
        })
        .catch(e => {
          this.success = false;
          this.showModal();
          console.log(e);
        });
      this.producto = {};
    },
    showModal() {
      this.$refs["my-modal"].show();
    },
    hideModal() {
      this.$refs["my-modal"].hide();
    }
  }
};
</script>

<style>
.form {
  width: 70%;
  margin-left: 15%;
}
</style>