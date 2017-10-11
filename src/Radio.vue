<template>
  <div>
    <v-radio-group v-model="dataset.selected" :mandatory="mandatory">
      <v-radio color="error" class="mb-4" :label="label" value=""></v-radio>
      <v-radio
        v-for="(item, i) in dataset.items"
        :label="item[inputText]"
        class="mb-2"
        color="primary"
        :value="item[inputValue]"
        :input-value="inputValue"
        hide-details
        :key="i"
      ></v-radio>
    </v-radio-group>
  </div>
</template>

<script>
  export default {
    name: 'radio',
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
      selected: '',
    },
    data () {
      return {
        dataset: {
          items: [],
          selected: ''
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
