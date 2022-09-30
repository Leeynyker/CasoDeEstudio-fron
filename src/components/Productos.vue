<template>
  <div class="maintable">
    <!-- <div class="">
      <button>Crear</button>
    </div> -->
    <v-dialog v-model="dialog" width="500">
      <template v-slot:activator="{ on, attrs }">
        <v-btn color="blue lighten-2" dark v-bind="attrs" v-on="on">
          Crear
        </v-btn>
      </template>

      <v-card>
        <form
          class="creatuser__form general__form"
          v-on:submit.prevent="postProduct()"
        >
          <v-text-field
            label="Producto"
            v-model="nProductoNombre"
          ></v-text-field>
          <v-text-field label="Tipo" v-model="nProductoTipo"></v-text-field>
          <v-text-field
            label="Propietario"
            v-model="nProductoTitular"
          ></v-text-field>
          <v-text-field label="Saldo" v-model="nProductoSaldo"></v-text-field>
          <v-text-field label="Cuota" v-model="nProductoCuota"></v-text-field>
          <v-btn type="submit" color="green lighten-2" dark v-bind="attrs" v-on="on">
            Crear
          </v-btn>
        </form>
      </v-card>
    </v-dialog>
    <v-data-table
      :headers="headers"
      :items="data"
      :items-per-page="5"
      class="elevation-1"
    >
      <template v-slot:item.action="{ item }">
        <v-dialog v-model="dialoge" width="500">
      <template v-slot:activator="{ on, attrs }">
        <v-btn @click="carge(item)" color="yellow lighten-2" dark v-bind="attrs" v-on="on">
          Editar
        </v-btn>
      </template>

      <v-card>
        <form
          class="creatuser__form general__form"
          v-on:submit.prevent="editProduct()"
        >
          <v-text-field
            label="Producto"
            v-model="mnProductoNombre"
          ></v-text-field>
          <v-text-field label="Tipo" v-model="mnProductoTipo"></v-text-field>
          <v-text-field
            label="Propietario"
            v-model="mnProductoTitular"
          ></v-text-field>
          <v-text-field label="Saldo" v-model="mnProductoSaldo"></v-text-field>
          <v-text-field label="Cuota" v-model="mnProductoCuota"></v-text-field>
          <v-btn type="submit" color="green lighten-2" dark v-bind="attrs" v-on="on">
            Editar
          </v-btn>
        </form>
      </v-card>
    </v-dialog>
        <v-btn color="red lighten-2"  @click="eliminarItem(item)" class="btnaccion">Eliminar</v-btn>
        
      </template>
    </v-data-table>
  </div>
</template>

<script>
const api_direction = "http://localhost:8000/";
export default {
  data() {
    return {
      data: [],
      dialog: false,
      dialoge: false,
      headers: [
        { text: "ID", value: "productoId" },
        {
          text: "Producto",
          align: "start",
          sortable: false,
          value: "productoNombre",
        },
        { text: "Tipo", value: "productoTipo" },
        { text: "Propietario", value: "productoTitular" },
        { text: "Saldo", value: "productoSaldo" },
        { text: "Cuota de manejo", value: "productoCuota" },
        { text: "Accion", value: "action" },
      ],

      nProductoNombre:"",
      nProductoTipo: "",
      nProductoTitular: "",
      nProductoSaldo: "",
      nProductoCuota: "",
      mnProductoID:"",
      mnProductoNombre:"",
      mnProductoTipo: "",
      mnProductoTitular: "",
      mnProductoSaldo: "",
      mnProductoCuota: "",
    };
  },
  methods: {
    postProduct() {
      let newProduct = new FormData();
      // newProduct.append('',)
      //nProductoNombre
      // nProductoTipo
      // nProductoTitular
      // nProductoSaldo
      // nProductoCuota
      newProduct.append("productoNombre", this.nProductoNombre),
        newProduct.append("productoTipo", this.nProductoTipo),
        newProduct.append("productoTitular", this.nProductoTitular),
        newProduct.append("productoSaldo", this.nProductoSaldo),
        newProduct.append("productoCuota", this.nProductoCuota);
      this.axios({
        method: "post",
        url: `${api_direction}products/`,
        data: newProduct,
        headers: {
          "Content-Type": "multipart/form-data",
        },
      })
        .then((response) => {
          console.log(response);
          
        })
        .catch((error) => {
          console.log(error);
        });
    },
    eliminarItem(item){
      this.axios({
        method: "delete",
        url: `${api_direction}products/${item.productoId}/`,
        // data: newProduct,
        headers: {
          "Content-Type": "multipart/form-data",
        },
      })
        .then((response) => {
          console.log(response);
          this.getProducts()          
        })
        .catch((error) => {
          console.log(error);
        });
    },
    carge(item){
      this.mnProductoID = item.productoId
      this.mnProductoNombre = item.productoNombre
      this.mnProductoTipo = item.productoTipo
      this.mnProductoTitular = item.productoTitular
      this.mnProductoSaldo = item.productoSaldo
      this.mnProductoCuota = item.productoCuota
    },
    editProduct(){
      let newProduct = new FormData();
      // newProduct.append('',)
      //nProductoNombre
      // nProductoTipo
      // nProductoTitular
      // nProductoSaldo
      // nProductoCuota
      newProduct.append("productoNombre", this.mnProductoNombre),
      newProduct.append("productoTipo", this.mnProductoTipo),
      newProduct.append("productoTitular", this.mnProductoTitular),
      newProduct.append("productoSaldo", this.mnProductoSaldo),
      newProduct.append("productoCuota", this.mnProductoCuota);
      this.axios({
        method: "patch",
        url: `${api_direction}products/${this.mnProductoID}/`,
        data: newProduct,
        headers: {
          "Content-Type": "multipart/form-data",
        },
      })
        .then((response) => {
          console.log(response);
          this.dialoge = false 
          this.getProducts()         
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getProducts() {
      this.axios({
        method: "get",
        url: `${api_direction}products/`,
        // data: busqueda,
        headers: {
          "Content-Type": "multipart/form-data",
        },
      })
        .then((response) => {
          console.log(response);
          this.data = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  mounted() {
    this.getProducts();
  },
};
</script>

<style scoped>
.maintable {
  width: 90%;
  margin: auto;
  margin-top: 50px;
}
.creatuser__form {
  width: 90%;
  margin: 10px auto;
  padding: 10px;
}
</style>