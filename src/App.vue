<template>
  <loading
    v-model:active="isLoading"
    :can-cancel="true"
    :on-cancel="onCancel"
    :is-full-page="fullPage"
     color="red"
     loader="dots"
  />
  <div class="d-flex flex-column min-vh-100 bg-light">
    <the-header></the-header>
    <div class="container-xxl">
      <app-pkdex-search @search-pkmn="getPkmn"></app-pkdex-search>
      <app-pkdex-grid
        v-if="pkmnData !== null && isModalVisible === false"
        :pkmn-data="pkmnData"
      ></app-pkdex-grid>
    </div>
    <the-footer></the-footer>
    <the-error-modal
      v-if="isModalVisible"
      :error-request="errorRequest"
      @close-modal="closeModal"
    ></the-error-modal>
  </div>
</template>

<script>
import TheHeader from './components/Layout/TheHeader.vue';
import AppPkdexSearch from './components/UI/AppPkdexSearch.vue';
import TheFooter from './components/Layout/TheFooter.vue';
import AppPkdexGrid from './components/UI/AppPkdexGrid.vue';
import getAxios from './components/Functionality/vue-pokedex-project';
import TheErrorModal from './components/Layout/TheErrorModal.vue';
import Loading from 'vue-loading-overlay';

export default {
  components: {
    TheHeader,
    AppPkdexSearch,
    TheFooter,
    AppPkdexGrid,
    TheErrorModal,
    Loading,
  },
  data() {
    return {
      axiosInstance: getAxios(),
      isLoading: false,
      pkmnData: null,
      isModalVisible: false,
      errorRequest: {
        errorTitle: '',
        errorMessage: '',
      },
    };
  },
  methods: {
    getPkmn(value) {
      this.isLoading = true;
      let url = `/${value.searchType}/${value.searchQuery}`;
      this.axiosInstance
        .get(url)
        .then((result) => {
          this.pkmnData = result;
        })
        .catch((error) => {
          this.errorRequest.errorMessage = error.message;
          this.errorRequest.errorTitle = error.response.data;
          this.isModalVisible = true;
        })
        .finally(() => {
          this.isLoading = false;
        });
    },
    closeModal() {
      this.isModalVisible = false;
    },
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
