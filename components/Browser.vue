<template>
  <div class="box browser">
    <b-button
      class="close-browser"
      type="is-danger is-light"
      icon-left="times"
    />
    <div v-html="code" />
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

<style>
.browser {
  position: relative;
}

.close-browser {
  position: absolute;
  top: 0;
  right: 0;
}
</style>
