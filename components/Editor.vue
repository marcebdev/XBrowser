<template>
  <prism-editor
    v-model="innerValue"
    class="my-editor"
    :highlight="highlighter"
    line-numbers
  />
</template>

<script>
// import Prism Editor
import { PrismEditor } from 'vue-prism-editor'
import 'vue-prism-editor/dist/prismeditor.min.css' // import the styles somewhere

// import highlighting library (you can use any library you want just return html string)
import { highlight, languages } from 'prismjs/components/prism-core'
import 'prismjs/components/prism-clike'
import 'prismjs/components/prism-javascript'
import 'prismjs/themes/prism-tomorrow.css' // import syntax highlighting styles

export default {
  components: { PrismEditor },
  props: {
    value: {
      type: String,
      default: '',
    },
  },
  computed: {
    innerValue: {
      get() {
        return this.value
      },
      set(newValue) {
        this.$emit('input', newValue)
      },
    },
  },
  methods: {
    highlighter() {
      return highlight(this.value, languages.js)
    },
  },
}
</script>

<!-- Global Styles -->
<style lang="scss">
.my-editor {
  textarea {
    outline: none !important;
  }
}
</style>

<style scoped>
.my-editor {
  padding: 15px 0px;

  background: hsl(0, 0%, 93%);

  font-family: Fira code, Fira Mono, Consolas, Menlo, Courier, monospace;
  font-size: 14px;
  line-height: 1.5;
}
</style>
