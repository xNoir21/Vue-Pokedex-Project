<template>
  <div class="container text-center pt-4">
    <div class="row">
      <div class="col-3" />
      <div class="col-6">
        <form>
          <fieldset>
            <legend>Search for a Pokémon!</legend>
            <div class="mb-3">
              <label class="form-label pt-2">Try it now!</label>
              <input
                type="text"
                class="form-control"
                placeholder="Insert a pokemon name or number"
              />
            </div>
            <div class="mb-3">
              <label class="form-label pt-2">Select Search Term</label>
              <select class="form-select">
                <option value="-1" selected="true" disabled hidden>
                  Select a option
                </option>
                <option value="pokemon">Pokémon General info</option>
                <option value="pokemon-species">Pokémon Species</option>
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
