<template>
  <ul class="categories-sidebar">
    <CategoriesSidebarItem
      v-for="category of categories"
      v-if="category.rules.length >= 1"
      :category="category"
      :config="config"
      :key="category.name"
      @change="emitChangeEvent"
    />
  </ul>
</template>

<script>
import CategoriesSidebarItem from './CategoriesSidebarItem.vue'
import { ruleCategories } from '../utils/eslint.js'

export default {
  name: 'CategoriesSidebar',

  components: { CategoriesSidebarItem },

  props: {
    config: {
      type: Object,
      default () {
        return { rules: {} }
      }
    }
  },

  computed: {
    categories () {
      return ruleCategories
    }
  },

  methods: {
    emitChangeEvent () {
      this.$emit('change')
    }
  }
}
</script>

<style lang="scss" scoped>
.categories-sidebar {
  list-style: none;
  margin: 0;
  padding: 0;
  overflow-x: hidden;
  overflow-y: scroll;
  border-right: 1px solid #CCC;
  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAQAAAAECAYAAACp8Z5+AAAAHElEQVQYV2O8e/fuf2VlZUYGKIAzMARgKjFUAABhrQgFvaGkawAAAABJRU5ErkJggg==) repeat;
}
</style>
