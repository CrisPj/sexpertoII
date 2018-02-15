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
					<b-input type="text" :value="regla" v-model="regla" placeholder="Regla" required>
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
		
		<div class="columns">
			<div class="column">
				
				<div class="card">
					<header class="card-header">
						<p class="card-header-title">
							Ver
						</p>
						
					</header>
					<div class="card-content">
						<div class="content">
							<a @click="isModalActive2 = true" class="button is-link">Ver Reglas</a>
							&nbsp; &nbsp; &nbsp; &nbsp;
						</div>
					</div>
					<footer class="card-footer">
						
					</footer>
				</div>
			</div>
			<br>
			<div class="column">
				<div class="card">
					<header class="card-header">
						<p class="card-header-title">
							Agregar
						</p>
						
					</header>
					<div class="card-content">
						<div class="content">
							<a @click="isModalActive = true" class="button is-success">Añadir más reglas a la base de conococimiento</a>
							&nbsp; &nbsp; &nbsp; &nbsp;
						</div>
					</div>
					<footer class="card-footer">
						
					</footer>
				</div>
				<br>
			</div>
		</div>
		<div class="card">
			<header class="card-header">
				<p class="card-header-title">
					Eliminar
				</p>
				
			</header>
			<div class="card-content">
				<div class="content">
					<a class="button is-warning">Borrar Reglas una a la vez</a>
					&nbsp; &nbsp; &nbsp; &nbsp;
					<a class="button is-danger">Borrar todas las  Reglas</a>
					&nbsp; &nbsp; &nbsp; &nbsp;
					<a class="button is-danger">Borrar todos los  Hechos</a>
				</div>
			</div>
			<footer class="card-footer">
			</footer>
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
          //await this.$axios.post('http://192.168.100.3:8080/addRegla',{"regla":this.regla})

          this.hecho = null
          this.getHechos()
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
	}
	}
}
</script>