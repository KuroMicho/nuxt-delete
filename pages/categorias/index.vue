<template>
  <div>
    <div class="row mt-3">
      <div class="col-xs-6">
        <h4>Categorias de Productos</h4>
      </div>
      <div class="col-xs-6 ml-2">
        <b-button variant="primary" href="/categorias/nuevo">Nuevo</b-button>
      </div>
    </div>

    <div class="row mt-2">
      <div class="col-sm-12">
        <b-table
          responsive
          striped
          hover
          :fields="fields"
          :items="items"
          id="categorias"
          :current-page="currentPage"
          :per-page="perPage"
        >
          <template slot="acciones" slot-scope="row">
            <b-button variant="success">Editar</b-button>

            <b-button
              variant="danger"
              type="button"
              @click="eliminar(row.item.id, row.index)"
            >Eliminar</b-button>
          </template>
        </b-table>

        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          aria-controls="categorias"
        ></b-pagination>
      </div>
    </div>
  </div>
</template>

<script>
import firebase, { db } from "../../services/firebase";


export default {
  asyncData() {
    let perPage = 2;
    return db
      .collection("categorias")
      .get()
      .then(async res => {
        let items = [];
        res.forEach(value => {
          items.push({
            id: value.id,

            ...value.data()
          });
        });

        return {
          items,
          currentPage: 1,
          rows: res.docs.length,
          perPage
        };
      });
  },
  data() {
    return {
      fields: [
        {
          key: "id",
          sortable: true
        },
        {
          key: "nombre",
          sortable: true
        },
        {
          key: "tipo",
          sortable: true
        },
        {
          key: "acciones",
          sortable: false
        }
      ]
    };
  },
  methods: {
    eliminar(id, index) {
      if (confirm("Estas seguro?")) {
        db.collection("categorias")
          .doc(id)
          .delete()
          .then(res => {
            this.items.splice(index, 1);
            alert("Categoria Eliminada");
          })
          .catch(function(error) {
            alert.error("Error al Eliminar Categoria;", error);
          });
      }
    }
  }
};
</script>
