<template>
  <v-card class="category elevation-1">
    <v-toolbar card class="transparent">
      <v-toolbar-title class="subheading">
        <v-icon class="accent--text" left v-html="icon"></v-icon>
        <span class="accent--text" v-html="label"></span>
      </v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn v-if="search" @click.stop="searchform.model = !searchform.model" icon><v-icon>search</v-icon></v-btn>
      <v-btn icon @click.stop="addform.model = !addform.model"><v-icon>{{ !addform.model ? 'keyboard_arrow_down' : 'keyboard_arrow_up' }}</v-icon></v-btn>
    </v-toolbar>

    <v-slide-y-transition>
      <v-card flat v-show="addform.model">
        <slot name="form" :props="{dataset, save: save }">
          <v-card-text>
            <v-text-field :error-messages="dataset.errors[inputValue]" hide-details v-model="dataset.item[inputValue]" :label="`Add ${label}`"></v-text-field>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn class="elevation-1 accent white--text" ripple @click.stop="save" v-html="addLabel"></v-btn>
          </v-card-actions>
        </slot>
      </v-card>
    </v-slide-y-transition>

    <v-slide-y-transition v-if="search">
      <v-card-actions v-show="searchform.model">
        <v-text-field
          solo
          v-model="searchform.query"
          append-icon="search" hide-details single-line></v-text-field>
      </v-card-actions>
    </v-slide-y-transition>

    <v-divider></v-divider>

    <v-card-text class="content--scrollable">
      <template v-if="multiple">
        <checkbox v-model="selected" :list="dataset.items" :input-text="inputText" :input-value="inputValue"></checkbox>
      </template>
      <template v-else>
        <radio
          :label="radioLabel"
          :list="dataset.items"
          :mandatory="mandatory"
          v-model="selected"
          :input-text="inputText"
          :input-value="inputValue"
        ></radio>
      </template>
    </v-card-text>

    <slot :items="selected">
      <v-divider></v-divider>
      <v-footer>
        <input type="hidden" :name="`${name}_string`" :value="JSON.stringify(selected)">
        <input type="hidden" :name="name" :value="selected">
      </v-footer>
    </slot>
  </v-card>
</template>

<script>
  import Radio from './Radio.vue'
  import Checkbox from './Checkbox.vue'

  export default {
    name: 'v-category',
    components: {
      Radio,
      Checkbox
    },
    model: {
      prop: 'content'
    },
    props: {
      addLabel: { type: String, default: 'Add' },
      content: null,
      errors: { type: Array, default: () => { return [] } },
      icon: { type: String, default: 'fa-leaf' },
      inputText: { type: String, default: 'name' },
      inputValue: { type: String, default: 'name' },
      label: { type: String, default: 'Category' },
      list: { type: Array, default: () => { return [] } },
      mandatory: { type: Boolean, default: false },
      multiple: { type: Boolean, default: false },
      name: { type: String, default: 'category' },
      radioLabel: { type: String, default: 'None' },
      search: { type: Boolean, default: false },
      urls: { type: Object, default: () => { return {} } },
    },
    data () {
      return {
        selected: null,
        searchform: {
          model: false,
          searchables: []
        },
        dataset: {
          items: [],
          item: { name: '' },
          errors: [],
        },
        addform: {
          model: false
        },
      }
    },
    methods: {
      save () {
        let self = this

        this.post().then((data) => {
          console.log("pooooooo", data)
          // if (! response.ok) {
          //   self.dataset.errors = data.item
          // }

          self.refresh()
          self.$emit('post', self.dataset, data.item)
        }, (data) => {
          console.log('err', data)
        })

        this.$emit('save', this.dataset)
      },

      refresh () {
        let self = this

        this.dataset.item = {}
        this.get()
        this.$emit('refresh', this.dataset)
      },

      get () {
        let self = this

        this.$http.get(this.urls.GET).then((response) => {
          let items = response.body
          self.dataset.items = items
          self.$emit('get', self.dataset, items)
        })
      },

      post () {
        let self = this

        return new Promise((resolve, reject) => {
          self.$http.post(self.urls.POST, self.dataset.item).then((response) => {
            let item = response.body
            resolve({item, response})
          }, (response) => {
            self.dataset.errors = response.body
            reject({response})
          })
        })
      }
    },
    mounted () {
      let self = this
      this.selected = this.content
      this.dataset.items = this.list

      this.dataset.item = this.dataset.items[0]
      for (let i in this.dataset.item) {
        this.dataset.item[i] = ""
      }

      this.dataset.errors = this.errors
      this.get()
    },
    watch: {
      // 'content': function (value) {
      //   this.selected = value
      // },
      'searchform.query': function (value) {
        this.$emit('search', value, this.list)
      },
      'content': function (value) {
        this.selected = value
      },
      'selected': function (value) {
        this.$emit('input', value)
      }
    }
  }
</script>

<style lang="scss" scoped>
  .content {
    &--scrollable {
      max-height: 200px;
      overflow-y:auto;
    }

    &--bordered {
      border-top: 1px solid rgba(0,0,0,0.05);
      border-bottom: 1px solid rgba(0,0,0,0.05)
    }
  }
</style>
