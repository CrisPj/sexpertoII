<template>
  <v-app>
    <v-navigation-drawer fixed temporary v-model="drawer" app>
      <v-list>
        <v-list-tile>
          <v-list-tile-content>
            <v-list-tile-title>
              <span>Menu</span>
            </v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
        <v-divider></v-divider>
        <v-list-tile router :to="item.to" :key="i" v-for="(item, i) in menuItems" exact>
          <v-list-tile-action>
            <v-icon v-html="item.icon"></v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title v-text="item.title"></v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>  
      </v-list>
    </v-navigation-drawer>
    <v-toolbar color="primary" dark app>
      <v-toolbar-side-icon @click="drawer = !drawer"></v-toolbar-side-icon>
      <v-toolbar-title v-text="title"></v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
    <v-content>
      <v-container>
          <nuxt />
          </v-container>
    </v-content>
    <v-footer color="primary" dark app>
      <span class="white--text text-xs-center">Python Team &copy; 2018 - Powered by Captain Morgan & Jack Daniel's</span>
    </v-footer>
  </v-app>
</template>

<script>
  export default {
    computed: {
      menuItems() {
        let menuItems = [{
          icon: 'home',
          title: 'Home',
          to: '/'
        },
        {
          icon: 'settings',
          title: 'Configuracion',
          to: '/configuracion'
        }, {
            icon: 'people',
                title: 'Acerca de',
                to: '/about'},
        ]
        return menuItems
      }
    },
    data() {
      return {
        clipped: true,
        drawer: false,
        fixed: false,
        items: undefined,
        title: 'Sistema Experto Neuronal - Desarrollos Inteligentes'
      }
    }
  }
</script>
