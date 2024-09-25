<template>
  <div class="d-flex flex-column min-vh-100 bg-light">
    <the-header></the-header>
    <div class="container-xxl">
      <app-pkdex-search></app-pkdex-search>
      <app-pkdex-grid></app-pkdex-grid>
    </div>
    <the-footer></the-footer>
  </div>
</template>

<script>
import TheHeader from './components/Layout/TheHeader.vue';
import AppPkdexSearch from './components/UI/AppPkdexSearch.vue';
import TheFooter from './components/Layout/TheFooter.vue';
import AppPkdexGrid from './components/UI/AppPkdexGrid.vue';

export default {
  components: {
    TheHeader,
    AppPkdexSearch,
    TheFooter,
    AppPkdexGrid,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
