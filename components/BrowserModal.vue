<template>
  <form class="modal-card" @submit.prevent="onSubmit()">
    <header class="modal-card-head">
      <h3 class="modal-card-title">Add Browser</h3>
    </header>
    <section class="modal-card-body">
      <b-field label="Browser Type">
        <b-select v-model="type" expanded>
          <option value="chrome">Chrome (Placeholder, runs native)</option>
          <option value="firefox">Firefox (Placeholder, runs native)</option>
          <option value="safari">Safari (Placeholder, runs native)</option>
          <option value="edge">Edge (Placeholder, runs native)</option>
          <option value="ie">
            Internet Explorer (Placeholder, runs native)
          </option>
        </b-select>
      </b-field>

      <b-field label="Browser Version">
        <b-select v-model="version" expanded>
          <option v-for="index in 10" :key="index" :value="version">
            {{ index }}
          </option>
        </b-select>
      </b-field>
    </section>
    <footer class="modal-card-foot">
      <b-button
        native-type="submit"
        label="Add Browser"
        type="is-primary"
        expanded
      />
    </footer>
  </form>
</template>

<script>
// Import Utils
import { nanoid } from 'nanoid/non-secure'

export default {
  data() {
    return {
      type: 'firefox',
      version: '1',
    }
  },
  methods: {
    onSubmit() {
      this.$emit('addBrowser', {
        id: nanoid(),
        type: this.type,
        version: this.version,
      })
      this.$parent.close()
    },
  },
}
</script>
