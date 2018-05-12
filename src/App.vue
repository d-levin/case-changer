<template>
  <div
    id="app"
    class="full-height"
  >
    <div class="grid-container">
      <div>
        <div>
          <label for="inputField">
            <strong>Enter value to convert:</strong>
          </label>
          <input
            v-model="value"
            id="inputField"
            type="text"
            class="full-width"
          >
        </div>
      </div>

      <div>
        <div
          v-for="(conversion, key) in conversions"
          class="conversion-item"
        >
          <label
            :for="key"
            class="conversion-item-label"
          >
            {{ conversion.label }}:
          </label>

          <input
            :value="conversion.value"
            :id="key"
            @click="copyToClipboard"
            type="text"
            class="conversion-item-input"
            readonly
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import * as changeCase from 'change-case'

const casesSorted = [
  'camelCase',
  'constantCase',
  'dotCase',
  'headerCase',
  'lowerCase',
  'lowerCaseFirst',
  'noCase',
  'paramCase',
  'pascalCase',
  'pathCase',
  'sentenceCase',
  'snakeCase',
  'swapCase',
  'titleCase',
  'upperCase',
  'upperCaseFirst'
]

export default {
  name: 'App',
  data () {
    return {
      value: '',
      conversions: casesSorted.reduce((aggregate, key) => {
        aggregate[key] = {
          label: changeCase[key](key),
          value: ''
        }
        return aggregate
      }, {})
    }
  },
  watch: {
    value (newValue) {
      Object.keys(this.conversions).forEach(key => {
        this.conversions[key].value = changeCase[key](newValue)
      })
    }
  },
  methods: {
    copyToClipboard (evt) {
      if (evt.target.value) {
        evt.target.select()
        document.execCommand('copy')
        alert(`[${evt.target.value}] copied to clipboard`)
      }
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  height: 100vh;
}

input {
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  border-radius: 3px;
  border: 1px solid rgba(0, 0, 0, 0.5);
  outline: none;
  padding: 0.625rem;
  color: black;
  transition: border-color 0.2s ease;
}

input:read-only {
  border-color: rgba(0, 0, 0, 0.2);
  color: rgba(0, 0, 0, 0.7);
}

input:focus {
  border-color: black;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.full-height {
  min-height: 100%;
}

.full-width {
  width: 100%;
}

.grid-container {
  padding: 1rem;
  display: grid;
  grid-template-rows: 30% auto;
  grid-row-gap: 1rem;
}

.conversion-item:not(:first-child) {
  margin-top: 1rem;
}

.conversion-item-label {
  display: inline-block;
  width: 180px;
}

.conversion-item-input {
  width: calc(100% - 180px - 5px);
}
</style>
