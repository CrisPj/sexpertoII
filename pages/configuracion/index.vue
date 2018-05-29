<template>

	<v-layout row>
		<v-flex xs12 sm24 offset>
			<v-card class="mb-4">
				<v-card-title primary-title>
					<div>
						<div class="headline">Desarrollos Inteligentes</div>
						<span class="grey--text">Instituto Tecnólogico de Celaya</span>
					</div>
				</v-card-title>
			</v-card>

			<v-card class="mb-4">
				<v-card-title primary-title>
					<div>
						<div class="headline">Configuración</div>
						<span class="grey--text">Esta sección esta dedicada a la configuracion del Sistema Experto Neuronal, tal como es el ingreso de reglas, asi como para entrenar la Red Neuronal</span>
					</div>
				</v-card-title>
			</v-card>

			<v-card class="mb-4">
				<v-card-title primary-title>
					<div>
						<div class="headline">Reglas</div>
					</div>
				</v-card-title>
				<div class="right">
					<v-btn color="success" @click.stop="isModalActive=true">Agregar Reglas</v-btn>
					<v-btn color="error" @click.native="eliminarReglas">Eliminar Reglas</v-btn>
					<v-btn color="info" @click.native="entrenar">Entrenar red</v-btn>
				</div>
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
						<th><v-btn color="error" @click.native="borrarRegla(regla.llave)">Borrar</v-btn></th>
					</tr>
					</tbody>
				</table>
				<v-card-actions>
				</v-card-actions>
			</v-card>
		</v-flex>


		<div>
			<v-dialog
					v-model="isModalActive"
					fullscreen
					hide-overlay
					transition="dialog-bottom-transition"
					scrollable>
				<v-card tile>
					<v-toolbar card dark color="primary">
						<v-btn icon dark @click.native="isModalActive = false">
							<v-icon>close</v-icon>
						</v-btn>
						<v-toolbar-title>Agregar Regla</v-toolbar-title>
						<v-spacer></v-spacer>
						<v-toolbar-items>

						</v-toolbar-items>
					</v-toolbar>
					<v-card-text class="px-4 pb-2 text-xs-center">
						<h3 class="headline mb-0">Agregar regla</h3>
						<div class="control">
							<v-text-field
									v-model="regla" type="text"
									label="Nueva regla"
									required
							></v-text-field>
						</div>
						<div>
							<v-btn color="success" v-on:click="guardarRegla()">Agregar</v-btn>
						</div>
					</v-card-text>
					<div style="flex: 1 1 auto;"></div>
				</v-card>
			</v-dialog>
		</div>
	</v-layout>



</template>
<script>


export default {
	mounted() {
this.getReglas();
},
methods: {
	async entrenar()
	{
		try {
			const { data } = await this.$axios.get('http://localhost:8080/entrenar')
			if (data == true)
			{
				alert("Red entrenada")
			}
		} catch (e)
		{
			console.log(e.message)
		}
	},
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
          this.getHechos()
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
    loading2: false,
	}

}}
</script>