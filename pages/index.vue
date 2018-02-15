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
              <h3>Sistema Experto Determinista.</h3>
              <p>
                El presente sistema experto presenta una serie de mejoras, con base en las observaciones hechas a las areas de oportunidad.
              </p>
            </div>
          </div>
        </article>
      </div>
    </div>

    <b-modal :active.sync="isModalActive">
      <form action="" v-on:submit.prevent="onSubmit">
        <div class="modal-card" style="width: auto">
          <header class="modal-card-head">
            <p class="modal-card-title">Agregar Hecho</p>
          </header>
          <section class="modal-card-body">
            <b-field label="Hecho">
              <b-input type="text" :value="hecho" v-model="hecho" placeholder="p" required>
              </b-input>
            </b-field>
          </section>
          <footer class="modal-card-foot">
            <button class="button is-primary" v-on:click="guardarHecho()">Submit</button>
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

                <h3 class="is-bold">Tabla de hechos</h3>

            <p>
              <small>En la presente tabla se introducen los hechos a trabajar sobre la base de conocimiento ya establecida.
              </small>
            </p>
            <div class="is-pulled-right">
              <a @click="isModalActive = true" class="button is-success">Agregar hecho</a>
              &nbsp; &nbsp;
              <a @click="eliminarHechos()" class="button is-danger">Eliminar hechos</a>
              &nbsp; &nbsp;
              <a @click="isModalActive2 = true" class="button is-link">Ver Reglas</a>
            </div>
            <table class="table">
              <thead>
              <tr >
                <th>Hecho</th>
                <th></th>
              </tr>
              </thead>
              <tbody>

              <tr v-for="regla in hechos" :key="regla">
                <th>{{regla}}</th>
                <th class="button is-danger" @click="borrarHecho(regla)">Borrar</th>
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
      this.getReglas();
      this.getHechos();
    },
    methods: {
      async eliminarHechos() {
        try {
          await this.$axios.get('http://192.168.100.3:8080/rmHechos')
          this.getHechos()
        } catch (e) {
          console.log(e.message)
        }
      },
      async borrarHecho(dato) {
        try {
          await this.$axios.post('http://192.168.100.3:8080/rmHecho',{"hecho":dato})
          this.getHechos()
        } catch (e) {
          console.log(e.message)
        }
      },
      async hacerInferencia(adelante)
      {
        try {
          if (adelante) {
            const {data} = await this.$axios.post('http://192.168.100.3:8080/adelante', {"meta": this.meta})
            this.justificacion = data
          }
          else {
            const {data} = await this.$axios.post('http://192.168.100.3:8080/atras', {"meta": this.meta})
            this.justificacion = data
          }
          
          this.getHechos()
        } catch (e) {
          console.log(e.message)
        }
      },
      async guardarHecho() {
        try {
          this.isModalActive = false;
          await this.$axios.post('http://192.168.100.3:8080/addHecho',{"hecho":this.hecho})
          this.hecho = null
          this.getHechos()
        } catch (e) {
          console.log(e.message)
        }
      },
      async getReglas() {
        try {
          const { data } = await this.$axios.get('http://192.168.100.3:8080/reglas')
          this.reglas = data
        } catch (e) {
          console.log(e.message)
        }
      },
      async getHechos() {
        try {
          const { data } = await this.$axios.get('http://192.168.100.3:8080/hechos')
          this.hechos = data
        } catch (e) {
          console.log(e.message)
        }
      }
    },
    data() {
      return {
        isModalActive: false,
        isModalActive2: false,
        hecho: null,
        regla: null,
        meta: null,
        reglas: undefined,
        hechos: undefined,
        justificacion: undefined
      }
    }
  }
</script>
