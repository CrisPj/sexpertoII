<template>
<section class="container">
  <div>
    <h1 class="title">
    Desarrollos Inteligentes.
    </h1>
    <h2 class="subtitle">
    Instituto Tecnlógico de Celaya
    </h2>
    <p>
      El presente sistema experto presenta una serie de mejoras, con base en las observaciones hechas a las areas de oportunidad
    </p>
    <br>

    <b-modal :active.sync="isModalActive">
      <form action="">
        <div class="modal-card" style="width: auto">
          <header class="modal-card-head">
            <p class="modal-card-title">Login</p>
          </header>
          <section class="modal-card-body">
            <b-field label="Hecho">
              <b-input
                type="text"
                :value="hecho"
                placeholder="p"
                required>
              </b-input>
            </b-field>

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
            <p><strong><h3>Tabla de hechos</h3></strong></p>
            <p><small>En la presente tabla se introducen los hechos a trabajar sobre
              la base de conocimiento ya establecida.
            </small></p>
            <a @click="isModalActive = true" class="button is-success is-pulled-right">Agregar hecho</a>
            <table class="table">
              <thead>
                <tr>
                  <th>ID</th>
                  <th>Hecho</th>
                  <th>Seleccion</th>
                  <th>Borrar</th>
                </tr>
              </thead>
              <tfoot>
              <tr>
                <th>ID</th>
                <th>Hecho</th>
                <th>Seleccion</th>
                <th>Borrar</th>
              </tr>
              </tfoot>
              <tbody>
                <!---
                <div v-for="regla in reglas">
                  <tr>
                    <th>{{regla.llave}}</th>
                    <th>{{regla.hecho}}</th>
                    <td>[]</td>
                    <td>X</td>
                  </tr>
                </div>
                -->
              </tbody>
            </table>
          </div>
          <nav class="level is-mobile">
            <div class="level-left">
              <a class="level-item">
                <span class="icon is-small"><i class="fas fa-reply"></i></span>
              </a>
              <a class="level-item">
                <span class="icon is-small"><i class="fas fa-retweet"></i></span>
              </a>
              <a class="level-item">
                <span class="icon is-small"><i class="fas fa-heart"></i></span>
              </a>
            </div>
          </nav>
        </div>
      </article>
    </div>
    <div class="box">
      <article class="media">
        <div class="media-left">
        </div>
        <div class="media-content">
          <div class="content">
            <p><strong><h3>Motor de Inferencia</h3></strong></p>
            <p><small>En la siguiente sección se puede ingresar la meta a llegar o bien dejar que el propio motor determine la meta.
            </small></p>
            <br>
            <div class="control">
              <input class="input" type="text" placeholder="Meta o dejar vacio para que el sistema infiera por si mismo.">
              <hr>
              <a class="button is-link is-primary">Encadenamiento hacia adelante</a>
              &nbsp; &nbsp; &nbsp; &nbsp;
              <a class="button is-info is-link">Encadenamiento hacia atras</a>
            </div>

          </div>
          <nav class="level is-mobile">
            <div class="level-left">
              <a class="level-item">
                <span class="icon is-small"><i class="fas fa-reply"></i></span>
              </a>
              <a class="level-item">
                <span class="icon is-small"><i class="fas fa-retweet"></i></span>
              </a>
              <a class="level-item">
                <span class="icon is-small"><i class="fas fa-heart"></i></span>
              </a>
            </div>
          </nav>
        </div>
      </article>
    </div>
    <div class="box">
      <article class="media">
        <div class="media-left">
        </div>
        <div class="media-content">
          <div class="content">
            <p><strong><h3>Modúlo de Justificación</h3></strong></p>
            <p><small>En esta sección se presentan las reglas tomadas por el sistema para llegar a una conclusión.
            </small></p>
            <br>
            <table>
              <thead>
                <tr>
                  <th>ID</th>
                  <th>Regla</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td></td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
          <nav class="level is-mobile">
            <div class="level-left">
              <a class="level-item">
                <span class="icon is-small"><i class="fas fa-reply"></i></span>
              </a>
              <a class="level-item">
                <span class="icon is-small"><i class="fas fa-retweet"></i></span>
              </a>
              <a class="level-item">
                <span class="icon is-small"><i class="fas fa-heart"></i></span>
              </a>
            </div>
          </nav>
        </div>
      </article>
    </div>
    <br>
  </div>
</section>
</template>
<script>
  export default {
    data() {
      return {
        isModalActive: false,
        hecho: null
      }
    }
  }
</script>
