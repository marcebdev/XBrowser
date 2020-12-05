<template>
  <div class="card">
    <header class="card-header">
      <h4 class="card-header-title">{{ browser.type }}</h4>

      <b-button
        class="close-browser"
        type="is-danger"
        icon-left="times"
        outlined
      />
    </header>
    <section class="card-content">
      <div v-html="code" />
    </section>
  </div>
</template>

<script>
export default {
  props: {
    browser: {
      type: Object,
      required: true,
    },
    code: {
      type: String,
      required: true,
    },
  },
}
</script>

<style scoped>
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-right: 0.5rem;
}

.card-header-title {
  text-transform: capitalize;
}
</style>
