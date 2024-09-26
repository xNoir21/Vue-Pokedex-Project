<template>
  <div class="container text-center">
    <!-- Grid Header -->
    <div class="row justify-content-md-center border">
      <div class="col-md-auto col-2" />
      <div class="col-md-auto col-8 border">
        <div class="row justify-content-md-center align-items-center border">
          <div class="col-8 border">
            <p class="text-center">Pokémon Name: {{ pkmnName }}</p>
          </div>
          <div class="col-4 border">
            <img
              :src="
                pkmnData.data.sprites.other['official-artwork']['front_default']
              "
              :alt="'Pokémon Sprite for: ' + pkmnName"
              width="198px"
              height="198px"
            />
          </div>
        </div>
      </div>
      <div class="col-md-auto col-2" />
    </div>
    <!-- Grid info -->
    <div class="row align-items-center border">
      <div class="col-3" />
      <div class="col-3">
        <p>Base Stats:</p>
        <ul
          class="list-group list-group-flush"
          v-for="(stat, idx) in pkmnData.data.stats"
          :key="idx"
        >
          <li class="list-group-item bg-light text-start">
            {{ pkmnStat(stat) }}
          </li>
        </ul>
      </div>
      <div class="col-3">
        <p>Pokemon Type:</p>
        <ul
          class="list-group list-group-flush"
          v-for="(type, idx) in pkmnData.data.types"
          :key="idx"
        >
          <li class="list-group-item bg-light text-start">
            {{ pkmnType(type) }}
          </li>
        </ul>
      </div>
      <div class="col-3" />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    pkmnData: Object,
  },
  mounted() {
    console.log(this.pkmnData);
  },
  computed: {
    pkmnName() {
      return (
        this.pkmnData.data.name[0].toUpperCase() +
        this.pkmnData.data.name.slice(1)
      );
    },
  },
  methods: {
    pkmnStat(stat) {
      let statArray = stat.stat.name.split('-');
      let statName = '';
      statArray.forEach((item) => {
        statName += item[0].toUpperCase() + item.slice(1) + ' ';
      });
      return statName + ': ' + stat['base_stat'];
    },
    pkmnType(type) {
      return type.type.name[0].toUpperCase() + type.type.name.slice(1);
    },
  },
};
</script>
