<template>
  <section class="container">

    <b-modal :active.sync="isModalActive">
      <form action="" v-on:submit.prevent="guardarVariable">
        <div class="modal-card" style="width: auto">
          <header class="modal-card-head">
            <p class="modal-card-title">Agregar Variable Linguistica</p>
          </header>
          <section class="modal-card-body">
            <b-field label="Nombre">
              <b-input type="text" :value="nombre" v-model="nombre" placeholder="nombre" required>
              </b-input>
            </b-field>
            <b-field label="Alias">
              <b-input type="text" :value="alias" v-model="alias" placeholder="alias" required>
              </b-input>
            </b-field>
          </section>
          <footer class="modal-card-foot">
            <button class="button is-primary">Submit</button>
          </footer>
        </div>
      </form>
    </b-modal>

    <div class="card">

        <div class="card-content">
            <div class="columns">
              <div class="column is-3">
                <div class="is-pulled-right">
                  <a @click="isModalActive = true" class="button is-success">Agregar Variable Linguistica</a>
                </div>
                <b-table
                  :data="vars"
                  :loading="loading"
                  :columns="columns"
                  :selected.sync="selected"
                  focusable>
                </b-table>
              </div>
              <div class="column">Vas a configurar {{ this.selected }}</div>
            </div>
        </div>
    </div>
  </section>
</template>
<script>
  export default {
    mounted() {
      this.getVars();
    },
    methods: {
      async borrarVar(dato) {
        try {
          await this.$axios.post('http://localhost:8080/rmHecho',{"hecho":dato})
          this.getVars()
        } catch (e) {
          console.log(e.message)
        }
      },
      async guardarVariable() {
        try {
          this.isModalActive = false;
          await this.$axios.post('http://localhost:8080/addVar',{"nombre":this.nombre, "alias":this.alias})
          this.nombre = null
          this.alias = null

        } catch (e) {
          console.log(e.message)
          this.$toast.open({
            duration: 5000,
            message: e.response.data.error,
            position: 'is-bottom',
            type: 'is-danger'
          })
        }
        this.getVars()
      },
      async getVars() {
        this.loading = true;
        try {
          const { data } = await this.$axios.get('http://localhost:8080/getVars')
          this.vars = data
          this.loading = false
        } catch (e) {
          console.log(e.message)
        }
      }
    },
    data() {
      return {
        isModalActive: false,
        nombre: null,
        alias: null,
        loading: true,
        selected: undefined,
        columns: [
        {
          field: 'id',
          label: 'ID',
          width: '40',
          numeric: true
        },
        {
          field: 'nombre',
          label: 'Nombre',
        },
        {
          field: 'alias',
          label: 'Alias',
        }
      ],
        vars: undefined,
        justificacion: undefined
      }
    }
  }
</script>
