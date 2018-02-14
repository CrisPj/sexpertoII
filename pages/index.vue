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
        <div class="media-left">
        </div>
        <div class="media-content">
          <div class="content">
            <p><strong><h3>Sistema Experto Determinista.</h3></strong></p>
            <p>
              El presente sistema experto presenta una serie de mejoras, con base en las observaciones hechas a las areas de oportunidad.
            </p>
          </div>
          
        </div>
      </article>
    </div>
    <b-modal :active.sync="isModalActive">
    <form action="">
      <div class="modal-card" style="width: auto">
        <header class="modal-card-head">
          <p class="modal-card-title">Agregar Hecho</p>
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
    <b-modal :active.sync="isModalActive2">
    <form action="">
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
              </tr>
            </thead>
            <tfoot>
            <tr>
              <th>ID</th>
              <th>Regla</th>
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
            <div class="is-pulled-right">
              <a @click="isModalActive = true" class="button is-success">Agregar hecho</a>
              &nbsp; &nbsp;
              <a @click="isModalActive2 = true" class="button is-link">Ver Reglas</a>
            </div>
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
isModalActive2: false,
hecho: null
}
}
}
</script>