<template>
  <section>

    <div class="container">
      <div class="mb-4 mt-3 text-xs-center">
        <h1 class="title">
          Home
        </h1>
      </div>
      
      <v-layout class="mb-5">
        <v-flex xs12>
          <v-card class="px-4 pb-2 text-xs-center">
            <v-card-title primary-title>
              <div>
                <h3 class="headline mb-0">Tabla de hechos</h3>
              </div>
            </v-card-title>
              <div>
                <v-btn flat color="success" @click.stop="isModalActive = true">Agregar hecho</v-btn>
                <v-btn flat color="error" @click="eliminarHechos()">Eliminar hechos</v-btn>
                <v-btn flat color="info" @click.stop="isModalActive2 = true">Ver Reglas</v-btn>
              </div>

              <table v-if="hechos && hechos.length > 0" class="table">
                  <thead>
                  <tr >
                    <th>Hecho</th>
                    <th>Opciones</th>
                  </tr>
                  </thead>
                  <tbody>
                    <tr v-for="regla in hechos" :key="regla">
                      <th>{{regla}}</th>
                      <th>
                        <v-tooltip bottom>
                          <v-btn slot="activator" flat icon color="error" @click="borrarHecho(regla)">
                            <v-icon>delete</v-icon>
                          </v-btn>
                          <span>Borrar</span>
                        </v-tooltip>
                      </th>
                    </tr>
                  </tbody>
              </table>

          </v-card>
        </v-flex>
      </v-layout>

      <v-layout class="mb-5">
        <v-flex xs12>
          <v-card class="px-4 pb-2 text-xs-center">
            <v-card-title primary-title>
              <div>
                <h3 class="headline mb-0">Motor de Inferencia</h3>
              </div>
            </v-card-title>

            <div class="control">
              <v-text-field
                v-model="meta" type="text"
                name="input-1"
                label="Ingresar meta para inferir"
              ></v-text-field>
            </div>

            <div>
              <v-btn flat color="success" @click="hacerInferencia(true)">Encadenamiento hacia adelante</v-btn>
              <v-btn flat color="info" @click="hacerInferencia(false)">Respuesta Neuronal</v-btn>
            </div>

          </v-card>
        </v-flex>
      </v-layout>

      <v-layout v-if="justificacion != null" class="mb-5">
        <v-flex xs12>
          <v-card class="px-4 pb-2 text-xs-center">
            <v-card-title primary-title>
              <div>
                <h3 class="headline mb-0">Modúlo de Justificación</h3>
              </div>
            </v-card-title>

            <table class="table">
                <thead>
                <tr>
                  <th>ID</th>
                  <th>Regla</th>
                </tr>
                </thead>
                <tbody>
                  <tr v-for="regla in justificacion" :key="regla">
                    <th>{{regla.llave}}</th>
                    <th>{{trad(regla.reglas)}}->{{regla.consecuente}}</th>
                  </tr>
                </tbody>
            </table>

          </v-card>
        </v-flex>
      </v-layout>

    </div>

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
            <v-toolbar-title>Agregar hecho</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-toolbar-items>
              <!-- <v-btn dark flat @click.native="isModalActive = false">Save</v-btn> -->
            </v-toolbar-items>
          </v-toolbar>
          <v-card-text class="px-4 pb-2 text-xs-center">
            <h3 class="headline mb-0">Agregar hecho</h3>
            <div class="control">
              <v-text-field
                v-model="hecho" type="text"
                label="Nuevo hecho"
                required
              ></v-text-field>
            </div>
            <div>
              <v-btn color="success" v-on:click="guardarHecho()">Agregar</v-btn>
            </div>
          </v-card-text>
          <div style="flex: 1 1 auto;"></div>
        </v-card>
      </v-dialog>

      <v-dialog
        v-model="isModalActive2"
        fullscreen
        hide-overlay
        transition="dialog-bottom-transition"
        scrollable>
        <v-card tile>
          <v-toolbar card dark color="primary">
            <v-btn icon dark @click.native="isModalActive2 = false">
              <v-icon>close</v-icon>
            </v-btn>
            <v-toolbar-title>Reglas</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-toolbar-items>
              <!-- <v-btn dark flat @click.native="isModalActive2 = false">Save</v-btn> -->
            </v-toolbar-items>
          </v-toolbar>
          <v-card-text class="px-4 pb-2 text-xs-center ">
            <h3 class="headline mb-0">Reglas</h3>
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
          </v-card-text>
          <div style="flex: 1 1 auto;"></div>
        </v-card>
      </v-dialog>
    </div>
    
  </section>
</template>
<script>
  export default {
    mounted() {
      this.getReglas();
      this.getHechos();
    },
    methods: {
      trad(reglas){
        let x = ''
        for (var i = 0; i < reglas.length; i++) {
          if(reglas[i] === '')
          {
          }
          else if (i==0)
          x = reglas[i]
          else
          x = x + '&' + reglas[i]
        }
        return x
      },
      async eliminarHechos() {
        try {
          await this.$axios.get('http://localhost:8080/rmHechos')
          this.getHechos()
        } catch (e) {
          console.log(e.message)
        }
      },
      async borrarHecho(dato) {
        try {
          await this.$axios.post('http://localhost:8080/rmHecho',{"hecho":dato})
          this.getHechos()
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
          
          this.getHechos()
        } catch (e) {
          console.log(e.message)
        }
      },
      async guardarHecho() {
        try {
          this.isModalActive = false;
          await this.$axios.post('http://localhost:8080/addHecho',{"hecho":this.hecho})
          this.hecho = null
          this.getHechos()
        } catch (e) {
          console.log(e.message)
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
      async getHechos() {
        try {
          const { data } = await this.$axios.get('http://localhost:8080/hechos')
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
        meta: '',
        reglas: undefined,
        hechos: undefined,
        justificacion: undefined
      }
    }
  }
</script>
