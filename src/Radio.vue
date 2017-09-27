<template>
  <div>
    <v-radio-group v-model="dataset.selected" :mandatory="mandatory">

      <v-radio color="primary" class="mb-4" :label="label" value=""></v-radio>
      <v-radio color="primary" class="mb-2" v-for="(item, i) in list"
        :label="item.name" :key="i" :value="item"></v-radio>

    </v-radio-group>
    <slot :item="dataset.selected"></slot>
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
      selected: { type: String, default: '' },
    },
    data () {
      return {
        dataset: {
          items: [],
          selected: {}
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
      },
      'dataset.selected': function (value) {
        this.$emit('input', value)
      }
    }
  }
</script>
