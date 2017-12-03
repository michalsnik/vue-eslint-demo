<template>
  <div class="app-header">
    <label class="show-fixed-code-label">
      <input
        type="checkbox"
        v-model="fixedCode"
      >
      Show fixed code
    </label>
    Playground for
    <a href="https://github.com/vuejs/eslint-plugin-vue#readme" target="_blank">eslint-plugin-vue</a>.
  </div>
</template>

<script>
export default {
  name: 'AppHeader',

  props: {
    showFixedCode: {
      type: Boolean,
      required: true
    }
  },

  computed: {
    fixedCode: {
      get () {
        return this.showFixedCode
      },
      set (val) {
        this.$emit('showFixedCode', val)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.app-header {
  padding: 8px;
  background-color: #A5D6A7;
  border-bottom: 1px solid #4CAF50;
  font-family: $ff-base;
  font-weight: bold;
}

.show-fixed-code-label {
  display: block;
  float: right;
  font-weight: normal;

  > input[type=checkbox] {
    vertical-align: middle;
  }
}
</style>
