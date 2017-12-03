<template>
  <li class="category">
    <label class="category__header">
      <input type="checkbox" v-model="shown" style="display:none">
      <md-icon :kind="shown ? 'expand_more' : 'chevron_right'"/>
      {{ category.name }}
      <span class="category__count">
        ({{ countChecked }} / {{ countAll }})
      </span>
    </label>
    <ul v-if="shown" class="category__rules">
      <li>
        <label class="category__rule">
          <div class="category__rule-checkbox">
            <input
              type="checkbox"
              v-model="allRules"
              :true-value="2"
              :false-value="0"
              @change="emitChangeEvent"
            >
          </div>
          <div class="category__rule-name">
            (all rules)
          </div>
        </label>
      </li>
      <li v-for="rule of category.rules"
          :key="rule.name"
          :title="rule.description"
      >
        <label class="category__rule">
          <div class="category__rule-checkbox">
            <input
              type="checkbox"
              v-model="config.rules[rule.name]"
              :true-value="2"
              :false-value="0"
              @change="emitChangeEvent"
            >
          </div>
          <div class="category__rule-name">
            {{ stripPrefix(rule.name) }}
          </div>
          <a class="category__rule-link" :href="url(rule.name)" target="_blank">
            <md-icon kind="launch" title="Open document"/>
          </a>
        </label>
      </li>
    </ul>
  </li>
</template>

<script>
import MdIcon from './MdIcon.vue'
import { getRuleUrl } from '../utils/eslint.js'

export default {
  name: 'CategoriesSidebarItem',

  components: { MdIcon },

  props: {
    category: {
      type: Object,
      default () {
        return { name: '', rules: [] }
      }
    },
    config: {
      type: Object,
      default () {
        return { rules: {} }
      }
    }
  },

  data () {
    return { shown: false }
  },

  computed: {
    allRules: {
      get () {
        const checked = this.category.rules.every(rule => this.config.rules[rule.name] === 2)
        return checked ? 2 : 0
      },
      set (value) {
        for (const rule of this.category.rules) {
          this.config.rules[rule.name] = value
        }
      }
    },

    countChecked () {
      return this.category.rules.filter(rule => this.config.rules[rule.name] === 2).length
    },

    countAll () {
      return this.category.rules.length
    }
  },

  methods: {
    stripPrefix (ruleId) {
      return ruleId.replace('vue/', '')
    },

    emitChangeEvent () {
      this.$emit('change')
    },

    url: getRuleUrl
  }
}
</script>

<style lang="scss" scoped>
.category {
  &__header {
    display: block;
    padding: 8px;
    background-color: #E8F5E9;
    border-bottom: 1px solid #4CAF50;
    font-weight: bold;
    cursor: pointer;
  }

  &__count {
    white-space: nowrap;
    font-weight: normal;
    color: gray;
  }

  &__rules {
    list-style: none;
    margin: 0;
    padding: 0;
    background-color: white;
  }

  &__rule {
    display: flex;
    align-items: center;
    padding: 4px;
    padding-left: 16px;
    border-bottom: 1px solid #E8F5E9;
  }

  &:last-child > &__rule {
    border-bottom: 1px solid #4CAF50;
  }

  &__rule-checkbox {
    flex-shrink: 0;
    padding-top: 4px; /* align to the rule name */
    padding-right: 4px;
  }
  &__rule-name {
    flex-grow: 1;
  }
  &__rule-link {
    display: block;
    flex-shrink: 0;
  }

  &__header:hover,
  &__rule:hover {
    background-image: linear-gradient(to bottom, rgba(0,0,0,0.065), rgba(0,0,0,0.0325) 67%, rgba(0,0,0,0.065));
  }
}
</style>
