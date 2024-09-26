<template>
  <div class="container text-center pt-4">
    <div class="row">
      <div class="col-3" />
      <div class="col-6">
        <form @submit.prevent="searchPkmnCheck">
          <fieldset>
            <legend>Search for a Pokémon!</legend>
            <!-- Search input -->
            <div class="mb-3">
              <label class="form-label pt-2">Try it now!</label>
              <input
                type="text"
                class="form-control"
                v-model="data.searchQuery"
                placeholder="Insert a pokemon name or number"
              />
            </div>
            <!-- Search type -->
            <div class="mb-3">
              <label class="form-label pt-2">Select Search Term</label>
              <select class="form-select" v-model="data.searchType">
                <option value="-1" disabled hidden>Select a option</option>
                <option value="pokemon">Pokémon General info</option>
                <option value="pokemon-species" disabled>
                  Pokémon Species
                </option>
              </select>
            </div>
            <button type="submit" class="btn btn-primary">Submit</button>
          </fieldset>
        </form>
      </div>
      <div class="col-3" />
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      data: {
        searchQuery: '',
        searchType: '-1',
      },
    };
  },
  methods: {
    searchPkmnCheck() {
      if (this.data.searchQuery === '' || this.data.searchType === '-1') {
        console.log('error empty search');
        return;
      }
      this.$emit('search-pkmn', this.data);
      this.data.searchQuery = '';
      this.data.searchType = '-1';
    },
  },
};
</script>
