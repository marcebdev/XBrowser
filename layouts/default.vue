<template>
  <div class="flex-layout">
    <b-navbar type="is-dark">
      <template slot="brand">
        <b-navbar-item tag="router-link" :to="{ path: '/' }">
          <h1 class="title has-text-white">Xbrowser</h1>
        </b-navbar-item>
      </template>
      <template slot="start"></template>

      <template slot="end"> </template>
    </b-navbar>

    <nuxt />
  </div>
</template>

<script>
export default {}
</script>

<style scoped>
.flex-layout {
  display: flex;
  flex-direction: column;
  height: 100vh;
  overflow: hidden;
}
</style>
