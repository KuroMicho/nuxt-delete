<template>
  <b-form @submit.prevent="guardarProducto">
    <div class="row mt-3">
      <div class="col-xs-6 col-sm-5 col-md-6">
        <h4>Nueva Categoria</h4>
      </div>
    </div>
    <div class="row">
      <div class="col-sm-12">

        <b-form-group label="Nombre Categoria:" label-for="nombre">
          <b-form-input
            id="nombre"
            type="text"
            required
            v-model="form.nombre"
            placeholder="Ingresa la Categoria"
          />
        </b-form-group>

        <b-form-group label="Tipo Categoria:" label-for="tipo">
          <b-form-input
            id="tipo"
            type="text"
            required
            v-model="form.tipo"
            placeholder="Ingresa el tipo del producto"
          />
        </b-form-group>

        
      </div>
    </div>

    <div class="row">
      <div class="col-xs-12 offset-sm-5">
          <b-spinner variant="primary" v-if="guardando"></b-spinner>
      </div>
    </div>

    <div class="row">
      <div class="col-xs-12 offset-sm-5">
        <b-button-toolbar>
          <b-button-group right class="mx-2">
            <b-button href="/categorias">Volver</b-button>
            <b-button variant="primary" type="submit" :disabled="guardando">Guardar</b-button>
          </b-button-group>
        </b-button-toolbar>
      </div>
    </div>
  </b-form>
</template>

<script>
import { db } from '../../services/firebase'

export default {
  data(){
    return {
      form: {
        nombre: '',
        tipo: '',
      },
      guardando: false,
    }
  },
  methods: {
    guardarProducto(){
      this.guardando = true
      db.collection('categorias').add(this.form).then(res => {
        this.$router.push({ path: "/categorias" })
      })
    }
  }
}
</script>
