<template>
  <div id="app">
    <v-app>
      <v-container fluid grid-list-lg>
        <v-layout row wrap>
          <v-flex sm4>
            <vue-component label="Tags" :list="[{id:4, icon:'fa-leaf',name:'Wololo'}, {id:1, icon:'edit',name:'Yu'},{id:2, icon:'perm_media',name:'Lo'}]"></vue-component>
          </v-flex>
        </v-layout>
      </v-container>
    </v-app>
  </div>
</template>

<script>
import Component from '../src/Component.vue'

export default {
  name: 'app',
  components: { 'vue-component': Component },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<style lang="scss">
  @import "~vuetify/dist/vuetify.min.css"
</style>
