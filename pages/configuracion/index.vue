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
					<v-btn color="success" @click.native="entrenar">Agregar Reglas</v-btn>

					<v-btn color="error" @click.native="eliminarReglas">Eliminar Reglas</v-btn>
					<v-btn color="info" @click.native="entrenar">Entrenar red</v-btn>
				</div>
				<v-data-table
						:headers="headers"
						:items="desserts"
						hide-actions
						class="elevation-1"
				>
					<template slot="index" slot-scope="regla">
						<td>{{ regla.llave}}</td>
						<td>{{ regla.reglas }}</td>
						<td>{{ regla.consecuente }}</td>
					</template>
				</v-data-table>
				<v-card-actions>
				</v-card-actions>
			</v-card>
		</v-flex>
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