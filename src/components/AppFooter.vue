<template>
  <div class="app-footer">
    <div
      class="version-item"
      v-for="(v, name) in versions"
      :key="name"
    >
      <a :href="'https://github.com/' + v.repo" target="_blank">{{ name }}</a>
      v{{ v.version }}
    </div>
  </div>
</template>

<script>
import versions from '../utils/versions.js'

export default {
  data () {
    return {
      versions
    }
  }
}
</script>

<style lang="scss" scoped>
.app-footer {
  display: flex;
  justify-content: flex-end;
  flex-wrap: wrap;
  flex-shrink: 0;
  border-top: 1px solid #CCC;
}

.version-item {
  flex-shrink: 0;
  margin-right: 8px;

  &:not(:last-child)::after {
    content: ","
  }
}
</style>
