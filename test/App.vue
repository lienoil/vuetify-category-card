<template>
  <div id="app">
    <v-app>
      <v-container fluid grid-list-lg>
        <v-layout row wrap>
          <v-flex sm4>
            <vue-component input-text="icon" :multiple="false" v-model="msg" icon="fa-tag" label="Category" :list="listRadio">
              <!-- <template scope="item">
                <span v-html="item"></span>
              </template> -->
            </vue-component>
            <span v-html="msg"></span>
          </v-flex>

          <v-flex sm4>
            <vue-component multiple input-value="id" v-model="msgs" :list="list" icon="fa-tags" label="Tags">
              <template slot="form" scope="{props}">
                <v-card-text>
                  <v-text-field hide-details v-model="props.dataset.item.id" label="Add ID"></v-text-field>
                  <v-text-field hide-details v-model="props.dataset.item.name" label="Add Name"></v-text-field>
                </v-card-text>
                <span v-html="props"></span>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn class="elevation-1 accent white--text" ripple @click.stop="props.save">Save</v-btn>
                </v-card-actions>
              </template>
            </vue-component>
            <span v-html="msgs"></span>
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
      list: [{id:100, icon:'fa-push',name:'Push'},{id:4, icon:'fa-tag',name:'Wololo'}, {id:1, icon:'edit',name:'Yu'},{id:2, icon:'perm_media',name:'Lo'}],
      listRadio: [{id:100, icon:'fa-push',name:'Push'},{id:4, icon:'fa-tag',name:'Wololo'}, {id:1, icon:'edit',name:'Yu'},{id:2, icon:'perm_media',name:'Lo'}],
      msg: 'Push',
      msgs: [4],
    }
  }
}
</script>

<style lang="scss">
  @import "~vuetify/dist/vuetify.min.css"
</style>
