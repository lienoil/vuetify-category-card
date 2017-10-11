<template>
  <div>
    <v-checkbox
      :key="i"
      :label="item[inputText]"
      :value="dataset.items[i][inputValue]"
      :input-value="inputValue"
      color="primary"
      hide-details
      v-for="(item, i) in dataset.items"
      v-model="dataset.selected"
    ></v-checkbox>
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
      inputValue: { type: String, default: 'name' },
      inputText: { type: String, default: 'name' },
      selected: null,
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
      this.dataset.selected = this.selected
    },
    watch: {
      'list': function (value) {
        this.dataset.items = value
      },
      'selected': function (value) {
        this.dataset.selected = value
      },
      'dataset.selected': function (value) {
        this.$emit('input', value)
      }
    }
  }
</script>
