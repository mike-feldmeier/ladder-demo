<template lang="pug">
  q-page(class="flex flex-center")
    .q-gutter-md
      h6 {{ selected }}
      q-select(v-model="model.a", :options="computedOptionsA", @input="clearModelToDepth(1)", emit-value, map-options)
        template(v-slot:no-option)
          q-item: q-item-section.text-italic.text-grey No Options Available
      q-select(v-model="model.b", :options="computedOptionsB", @input="clearModelToDepth(2)", emit-value, map-options)
        template(v-slot:no-option)
          q-item: q-item-section.text-italic.text-grey No Options Available
      q-select(v-model="model.c", :options="computedOptionsC", @input="clearModelToDepth(3)", emit-value, map-options)
        template(v-slot:no-option)
          q-item: q-item-section.text-italic.text-grey No Options Available
      q-select(v-model="model.d", :options="computedOptionsD", @input="clearModelToDepth(4)", emit-value, map-options)
        template(v-slot:no-option)
          q-item: q-item-section.text-italic.text-grey No Options Available
      q-select(v-model="model.e", :options="computedOptionsE", emit-value, map-options)
        template(v-slot:no-option)
          q-item: q-item-section.text-italic.text-grey No Options Available
</template>

<script>
import options from '@/assets/options'

// Make a key "1:2:3:4:5" out of all values that aren't undefined...
const makeKey = (...values) => values.filter(v => v !== undefined).join(':')

// Match the given key against a key generated from the given model to the given depth...
const matchFilterKey = (key, model, depth = 0) => key === makeKey(...Object.values(model).slice(0, depth))

export default {
  name: 'HelloWorld',
  data () {
    return {
      model: {
        a: undefined, 
        b: undefined, 
        c: undefined, 
        d: undefined, 
        e: undefined
      },
      selected: ''
    }
  },
  computed: {
    computedOptionsA () {
      return options.a
    },
    computedOptionsB () {
      const result = options.b.filter(o => matchFilterKey(o.filterKey, this.model, 1))
      if(result.length === 1) { this.model.b = result[0].value}
      return result
    },
    computedOptionsC () {
      const result = options.c.filter(o => matchFilterKey(o.filterKey, this.model, 2))
      if(result.length === 1) { this.model.c = result[0].value}
      return result
    },
    computedOptionsD () {
      const result = options.d.filter(o => matchFilterKey(o.filterKey, this.model, 3))
      if(result.length === 1) { this.model.d = result[0].value}
      return result
    },
    computedOptionsE () {
      const result = options.e.filter(o => matchFilterKey(o.filterKey, this.model, 4))
      if(result.length === 1) { this.model.e = result[0].value}
      return result
    }
  },
  methods: {
    clearModelToDepth (depth = 0) {
      Object.keys(this.model).slice(depth).forEach(k => this.model[k] = undefined)
    }
  },
  watch: {
    'model.e': function(value) {
      // This is where we would update the Vuex store to register the filter instead of just updating the display...
      this.selected = value !== undefined ? JSON.stringify(this.model) : ''
    }
  }
}
</script>

<style lang="stylus">
  .q-select
    width 20rem
</style>
