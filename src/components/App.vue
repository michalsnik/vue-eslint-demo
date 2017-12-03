<template>
  <div class="app">
    <AppHeader
      class="app__header"
      :show-fixed-code="showFixedCode"
      @showFixedCode="(val) => showFixedCode = val"
    />

    <div class="app__body">
      <CategoriesSidebar
        class="app__sidebar"
        :config="config"
        @change="onConfigChange"
      />
      <div class="app__main">
        <CodeEditor
          class="app__editor"
          :code="code"
          :messages="messages"
          :fixed-code="fixedCode"
          :fixed-messages="fixedMessages"
          :show-fixed-code="showFixedCode"
          @edit="onEdit"
          @initialize.once="onEditorInitialize"
        />
        <MessageList class="app__errors" :messages="messages" />
      </div>
    </div>

    <AppFooter />
  </div>
</template>

<script>
import AppHeader from './AppHeader.vue'
import AppFooter from './AppFooter.vue'
import CodeEditor from './CodeEditor.vue'
import MessageList from './MessageList.vue'
import CategoriesSidebar from './CategoriesSidebar.vue'
import { linter } from '../utils/eslint.js'
import { deserializeState, serializeState } from '../utils/state.js'

export default {
  name: 'App',

  components: {
    AppHeader,
    AppFooter,
    CodeEditor,
    MessageList,
    CategoriesSidebar
  },

  data () {
    return deserializeState(window.location.hash.slice(1))
  },

  computed: {
    messages () {
      try {
        return linter.verify(this.code, this.config, 'vue-eslint-demo.vue')
      } catch (err) {
        return [{
          fatal: true,
          severity: 2,
          message: err.message,
          line: 1,
          column: 0
        }]
      }
    },

    fixResult () {
      try {
        return linter.verifyAndFix(this.code, this.config, 'vue-eslint-demo.vue')
      } catch (err) {
        return {
          output: this.code,
          messages: [{
            fatal: true,
            severity: 2,
            message: err.message,
            line: 1,
            column: 0
          }]
        }
      }
    },

    fixedCode () {
      return this.fixResult.output
    },

    fixedMessages () {
      return this.fixResult.messages
    }
  },

  mounted () {
    window.addEventListener('hashchange', this.onUrlHashChange)
  },

  beforeDestroy () {
    window.removeEventListener('hashchange', this.onUrlHashChange)
  },

  methods: {
    onEdit (code) {
      this.code = code
      this.applyUrlHash()
    },

    onEditorInitialize () {
      window.MainContent.show()
    },

    onConfigChange () {
      this.applyUrlHash()
    },

    onUrlHashChange () {
      const newSerializedState = window.location.hash.slice(1)
      const oldSerializedState = serializeState(this.$data)
      if (newSerializedState !== oldSerializedState) {
        Object.assign(this.$data, deserializeState(newSerializedState))
      }
    },

    applyUrlHash () {
      window.location.hash = `#${serializeState(this.$data)}`
    }
  }
}
</script>

<style src="../styles/base.scss" lang="scss"></style>
<style lang="scss" scoped>
.app {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;

  &__header {
    flex-shrink: 0;
  }

  &__body {
    display: flex;
    flex-grow: 1;
    min-height: 0;
  }

  &__sidebar {
    width: calc(25% - 1px);
  }

  &__main {
    display: flex;
    flex-direction: column;
    flex-grow: 1;
  }

  &__editor {
    height: calc(75% - 1px);
    flex-grow: 1;
    border-bottom: 1px solid #CCC;
  }

  &__errors {
    height: 25%;
  }
}
</style>
