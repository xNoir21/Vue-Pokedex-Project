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
        class="pt-2"
        v-if="pkmnData !== null && isModalVisible === false"
        :pkmn-data="pkmnData"
      ></app-pkdex-grid>
      <app-pkdex-homepage-grid
        v-else-if="pkmnDataList.length !== 0"
        class="pt-2 pb-2"
        :pkmn-data-list="pkmnDataList"
        @load-pkmn="getPkmnList"
        @load-pkmn-details="getPkmn"
      >
      </app-pkdex-homepage-grid>
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
import TheFooter from './components/Layout/TheFooter.vue';
import TheErrorModal from './components/Layout/TheErrorModal.vue';
import AppPkdexSearch from './components/UI/AppPkdexSearch.vue';
import AppPkdexGrid from './components/UI/AppPkdexGrid.vue';
import AppPkdexHomepageGrid from './components/UI/AppPkdexHomepageGrid.vue';
import getAxios from './components/Functionality/vue-pokedex-project';
import Loading from 'vue-loading-overlay';

export default {
  components: {
    TheHeader,
    AppPkdexSearch,
    TheFooter,
    AppPkdexGrid,
    TheErrorModal,
    Loading,
    AppPkdexHomepageGrid,
  },
  data() {
    return {
      axiosInstance: getAxios(),
      isLoading: false,
      pkmnData: null,
      pkmnDataList: [],
      endIndex: 0,
      pkmnDataListLimit: 10,
      isModalVisible: false,
      errorRequest: {
        errorTitle: '',
        errorMessage: '',
      },
    };
  },
  mounted() {
    this.getPkmnList();
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
    getPkmnList() {
      this.isLoading = true;
      let url = `pokemon?offset=${this.endIndex}&limit=${this.pkmnDataListLimit}`;
      this.axiosInstance
        .get(url)
        .then(async (result) => {
          for (let i = 0; i < result.data.results.length; i++) {
            let pkmn = result.data.results[i];
            let pkmnData = await this.axiosInstance.get(pkmn.url);
            let pkmnList = {
              name: pkmn.name,
              iconUrl: pkmnData.data.sprites.other.showdown.front_default,
              type: pkmnData.data.types,
              id: pkmnData.data.id,
            };
            this.pkmnDataList.push(pkmnList);
          }
        })
        .catch((error) => {
          this.errorRequest.errorMessage = error.message;
          this.errorRequest.errorTitle = error.response.data;
          this.isModalVisible = true;
        })
        .finally(() => {
          this.endIndex += this.pkmnDataListLimit;
          this.isLoading = false;
        });
    },
    closeModal() {
      this.isModalVisible = false;
    },
  },
};
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
