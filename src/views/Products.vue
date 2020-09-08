<template>
  <div class="products">
    <h1>Consulta De Productos</h1>
    <div class="search-wrapper">
      <input type="text" v-model="search" placeholder="Buscar..." />
      <b-button size="sm" class="my-2 my-sm-0" @click="filteredList">Buscar</b-button>
      <b-button size="sm" class="my-2 my-sm-0" @click="clearList">Cancelar</b-button>
    </div>
    <br />
    <template>
      <div>
        <table class="table">
          <thead>
            <tr>
              <th scope="col">Descripción</th>
              <th scope="col">Stock</th>
              <th scope="col">Precio de Venta</th>
              <th scope="col">Proveedor</th>
              <th scope="col">Sucursal</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in productos" :key="index._id">
              <th scope="row">{{item.Descripcion}}</th>
              <td>{{item.Stock}}</td>
              <td>{{item.PrecioVenta}}</td>
              <td>{{item.Proveedor}}</td>
              <td>{{item.Sucursal}}</td>
              <b-button
                class="btn btn-primary btn-sm mx-2"
                @click="showModalUpdate(item)"
              >Actualizar</b-button>
              <b-button class="btn-danger btn-sm mx-2" @click="showModalAlert(item._id)">Eliminar</b-button>
            </tr>
          </tbody>
        </table>
      </div>

      <div>
        <b-modal ref="my-modal" hide-footer title>
          <div class="d-block text-center">
            <h3 v-if="success == '1'">Producto eliminado exitosamente.</h3>
            <h3 v-if="success == '2'">Producto no pudo ser eliminado.</h3>
            <h3 v-if="success == '3'">Desea eliminar el producto?</h3>
          </div>
          <b-button
            v-if="success == '3'"
            class="mt-3"
            variant="danger"
            block
            @click="eliminarProducto(productId)"
          >Si</b-button>
          <b-button v-if="success == '3'" class="mt-3" variant="success" block @click="hideModal">No</b-button>
          <b-button
            v-if="success != '3'"
            class="mt-3"
            variant="success"
            block
            @click="hideModal"
          >Entendido</b-button>
        </b-modal>
      </div>

      <div>
        <b-modal ref="modalUpdate" hide-footer title>
          <div class="d-block text-center">
            <h1>Actualizar Producto</h1>
            <div class="form">
              <b-form @submit.prevent="updateProduct(producto)">
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
                <b-button class="btn-sm btn-block btn-success" type="submit">Actualizar</b-button>
              </b-form>
            </div>
          </div>
          <b-button class="mt-3" variant="success" block @click="hideModalUpdate">Cancelar</b-button>
        </b-modal>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  data() {
    return {
      productos: [],
      producto: {},
      success: "1",
      productId: "",
      search: ""
    };
  },
  created() {
    this.listarPacientes();
  },
  methods: {
    listarPacientes() {
      this.axios
        .get("/productos")
        .then(response => {
          this.productos = response.data.productos;
          this.productosTemporales = this.productos;
          console.log(this.productos);
        })
        .catch(e => {
          console.log("error" + e);
        });
    },
    showModalAlert(id) {
      this.success = "3";
      this.productId = id;
      this.showModal();
    },
    eliminarProducto(id) {
      this.axios
        .delete(`/productos/${id}`)
        .then(res => {
          this.success = "1";
          this.showModal();
          console.log("Res" + res);
          this.productId = "";
          this.listarPacientes();
        })
        .catch(e => {
          this.success = "2";
          this.showModal();
          console.log("error" + e);
          this.productId = "";
        });
    },
    showModal() {
      this.$refs["my-modal"].show();
    },
    hideModal() {
      this.$refs["my-modal"].hide();
    },
    showModalUpdate(product) {
      this.producto = product
      this.productId = product._id;
      this.$refs["modalUpdate"].show();
    },
    hideModalUpdate() {
      this.$refs["modalUpdate"].hide();
    },
    updateProduct() {
      this.axios
        .put("/productos", this.producto)
        .then(res => {
          console.log(res);
          this.hideModalUpdate();
          this.producto = {};
          this.productId = '';
        })
        .catch(e => {
          this.hideModalUpdate();
          this.producto = {};
          this.productId = '';
          console.log(e);
        });
    }
  }
};
</script>