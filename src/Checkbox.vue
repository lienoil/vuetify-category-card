<template>
  <div>
    <v-checkbox v-model="dataset.selected" hide-details color="primary" v-for="(item, i) in list"
      :label="item.name" :key="i" :value="item"></v-checkbox>

    <slot :item="dataset.selected"></slot>
  </div>
</template>

<script>
  export default {
    name: 'checkbox',
    model: {
      prop: 'selected'
    },
    props: {
      list: { type: Array, default: () => { return [] } },
      label: { type: String, default: 'None' },
      mandatory: { type: Boolean, default: false },
      multiple: { type: Boolean, default: false },
      selected: { type: String, default: '' },
    },
    data () {
      return {
        dataset: {
          items: [],
          selected: []
        },
        searchform: {
          model: false
        }
      }
    },
    mounted () {
      this.dataset.items = this.list
    },
    watch: {
      'list': function (value) {
        this.dataset.items = value
      }
    }
  }
</script>
