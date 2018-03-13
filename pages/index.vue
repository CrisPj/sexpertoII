<template>
  <section class="container">
    <div>

      <h1 class="title">
        Desarrollos Inteligentes.
      </h1>
      <h2 class="subtitle">
        Instituto Tecnlógico de Celaya
      </h2>
      <div class="box">
        <article class="media">
          <div class="media-left"></div>
          <div class="media-content">
            <div class="content">
              <h3>Difuso.</h3>
              <p>
                Esta bien difuso
              </p>
            </div>
          </div>
        </article>
      </div>
    </div>

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
    <b-modal :active.sync="isModalActive2">
      <form >
        <div class="modal-card" style="width: auto">
          <header class="modal-card-head">
            <p class="modal-card-title">Ver Reglas</p>
          </header>
          <section class="modal-card-body">
            <table class="table">
              <thead>
              <tr>
                <th>ID</th>
                <th>Regla</th>
                <th>Consecuente</th>
              </tr>
              </thead>
              <tbody>
              <tr v-for="(regla,index) in reglas" :key="index">
                <td>{{regla.llave}}</td>
                <td>{{regla.reglas}}</td>
                <td>{{regla.consecuente}}</td>
              </tr>
              </tbody>
            </table>
          </section>
          <footer class="modal-card-foot">
            <button class="button is-primary">Submit</button>
          </footer>
        </div>
      </form>
    </b-modal>
    <div class="box">
      <article class="media">
        <div class="media-left">
        </div>
        <div class="media-content">
          <div class="content">

                <h3 class="is-bold">Variables linguisticas</h3>

            <div class="is-pulled-right">
              <a @click="isModalActive = true" class="button is-success">Agregar Variable Linguistica</a>
              &nbsp; &nbsp;
              <a @click="eliminarHechos()" class="button is-danger">Eliminar variables</a>
            </div>
            <table class="table">
              <thead>
              <tr >
                <th>Nombre Variable</th>
                <th>Alias</th>
                <th></th>
              </tr>
              </thead>
              <tbody>

              <tr v-for="vari in vars" :key="vari.id">
                <th>{{vari.nombre}}</th>
                <th>{{vari.alias}}</th>
                <th class="button is-danger" @click="borrarHecho(vari.id)">Borrar</th>
              </tr>

              </tbody>
            </table>
          </div>

        </div>
      </article>
    </div>
    <div class="box">
      <article class="media">
        <div class="media-left">
        </div>
        <div class="media-content">
          <div class="content">

                <h3>Motor de Inferencia</h3>
            <p>
              <small>En la siguiente sección se puede ingresar la meta a llegar o bien dejar que el propio motor determine
                la meta.
              </small>
            </p>
            <br/>
            <div class="control">
              <input class="input" v-model="meta" type="text" placeholder="Meta o dejar vacio para que el sistema infiera por si mismo.">
              <hr/>
              <a class="button is-link is-primary" @click="hacerInferencia(true)">Encadenamiento hacia adelante</a>
              &nbsp; &nbsp; &nbsp; &nbsp;
              <a class="button is-info is-link" @click="hacerInferencia(false)">Encadenamiento hacia atras</a>
            </div>
          </div>

        </div>
      </article>
    </div>
    <div class="box">
      <article class="media">
        <div class="media-left">
        </div>
        <div class="media-content">
          <div class="content">
            <h3>Modúlo de Justificación</h3>
            <p>
              En esta sección se presentan las reglas tomadas por el sistema para llegar a una conclusión.
            </p>
            <br>
            <table>
              <thead>
              <tr>
                <th>ID</th>
                <th>Regla</th>
              </tr>
              </thead>
              <tbody>
              <tr v-for="regla in justificacion" :key="regla">
                <th>{{regla}}</th>
                <th>Nada</th>
              </tr>
              </tbody>
            </table>
          </div>

        </div>
      </article>
    </div>
    <br />
  </section>
</template>
<script>
  export default {
    mounted() {
//      this.getReglas();
      this.getVars();
    },
    methods: {
      async eliminarHechos() {
        try {
          await this.$axios.get('http://localhost:8080/rmHechos')
          this.getVars()
        } catch (e) {
          console.log(e.message)
        }
      },
      async borrarHecho(dato) {
        try {
          await this.$axios.post('http://localhost:8080/rmHecho',{"hecho":dato})
          this.getVars()
        } catch (e) {
          console.log(e.message)
        }
      },
      async hacerInferencia(adelante)
      {
        try {
          if (adelante) {
            const {data} = await this.$axios.post('http://localhost:8080/adelante', {"meta": this.meta})
            this.justificacion = data
          }
          else {
            const {data} = await this.$axios.post('http://localhost:8080/atras', {"meta": this.meta})
            this.justificacion = data
          }

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
          this.getVars()
        } catch (e) {
          this.$toast.open({
            duration: 5000,
            message: e.response.data.error,
            position: 'is-bottom',
            type: 'is-danger'
          })
        }
      },
      async getReglas() {
        try {
          const { data } = await this.$axios.get('http://localhost:8080/reglas')
          this.reglas = data
        } catch (e) {
          console.log(e.message)
        }
      },
      async getVars() {
        try {
          const { data } = await this.$axios.get('http://localhost:8080/getVars')
          this.vars = data
        } catch (e) {
          console.log(e.message)
        }
      }
    },
    data() {
      return {
        isModalActive: false,
        isModalActive2: false,
        nombre: null,
        alias: null,
        regla: null,
        meta: null,
        reglas: undefined,
        vars: undefined,
        justificacion: undefined
      }
    }
  }
</script>
