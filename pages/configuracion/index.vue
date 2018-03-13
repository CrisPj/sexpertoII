<template>
<section class="container">
	<div>
		<h1 class="title">
		Desarrollos Inteligentes.
		</h1>
		<h2 class="subtitle">
		Instituto Tecnlógico de Celaya
		</h2>
		<br>
		<div class="box">
			<article class="media">
				<div class="media-left">
				</div>
				<div class="media-content">
					<div class="content">
						<h3>Configuración</h3>
						<p>Esta sección esta dedicada a la configuración del sistema experto, aspectos relacionados con la Base de Conocimiento, Base de Hechos, ádemas de proporcionar una vista sobre el conococimiento con el que se trabaja.
						</p>
						<br>
					</div>
				</div>
			</article>
		</div>
		<b-modal :active.sync="isModalActive">
		<form action="" v-on:submit.prevent="onSubmit">
			<div class="modal-card" style="width: auto">
				<header class="modal-card-head">
					<p class="modal-card-title">Agregar Regla</p>
				</header>
				<section class="modal-card-body">
					<b-field label="Regla">
					<b-input type="text" :value="regla" v-model="regla" placeholder="Regl formada: Antecendente Operador antecendete -> consecuente" required>
					</b-input>
					</b-field>
				</section>
				<footer class="modal-card-foot">
					<button class="button is-primary" v-on:click="guardarRegla()">Submit</button>
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

                <h3 class="is-bold">Reglas</h3>

            <div class="is-pulled-right">
              <a @click="isModalActive = true" class="button is-success">Agregar Reglas</a>
              &nbsp; &nbsp;
              <a @click="eliminarReglas()" class="button is-danger">Eliminar Reglas</a>

            </div>
            <table class="table">
              <thead>
              <tr >
                <th>Llave</th>
                <th>Reglas</th>
                <th>Consecuente</th>
                <th></th>
              </tr>
              </thead>
              <tbody>

              <tr v-for="regla in reglas" :key="regla">
                <th>{{regla.llave}}</th>
                <th> {{regla.reglas}}</th>
                <th> {{regla.consecuente}}</th>
                <th class="button is-danger" @click="borrarRegla(regla.llave)">Borrar</th>
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
	mounted() {
this.getReglas();
},
methods: {

	async getReglas() {
	try {
		//const { data } = await this.$axios.get('http://localhost:8080/reglas')
		const { data } = await this.$axios.get('http://localhost:8080/reglas')
		this.reglas = data
	} catch (e) {
		console.log(e.message)
	}
	},
	async guardarRegla() {
        try {
          this.isModalActive = false;
          await this.$axios.post('http://localhost:8080/addRegla',{"regla":this.regla})
          //await this.$axios.post('http://localhost:8080/addRegla',{"regla":this.regla})

          this.regla = null
          this.getReglas()
        } catch (e) {
          console.log(e.message)
        }
      },
      async eliminarHechos() {
        try {
          await this.$axios.post('http://localhost:8080/rmHechos')
          this.getVars()
        } catch (e) {
          console.log(e.message)
        }
      },
      async eliminarReglas() {
        try {
          await this.$axios.post('http://localhost:8080/rmReglas')
          this.getReglas()
        } catch (e) {
          console.log(e.message)
        }
      },
      async borrarRegla(dato) {
        try {
          await this.$axios.post('http://localhost:8080/rmRegla',{"llave":dato})
          this.getReglas()
        } catch (e) {
          console.log(e.message)
        }
      },
},
data() {
return {
	isModalActive2: false,
	reglas: undefined,
	isModalActive: false,
	regla: null,
    hechos: undefined,
	}
	}
}
</script>
