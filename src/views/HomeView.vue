<template>
  <div class="container">
    <main>
      <div class="pt-4 mb-8 relative text-white position-relative">
        <input
          v-model="searchQuery"
          @input="getSearchResult()"
          type="text"
          placeholder="Search for a city or state"
          class="search w-100 text-white py-2 px-1 bg-transparent border-0 border-bottom"
        />
        <ul class="result-menu" v-if="mapBoxSearchResult">
          <p class="m-0" v-if="searchError">
            Sorry, something went wrong, please try again.
          </p>
          <p class="m-0" v-if="!serverError && mapBoxSearchResult.length === 0">
            No results match your query, try a different term
          </p>
          <template v-else>
            <li
              v-for="item in mapBoxSearchResult"
              :key="item.id"
              @click="previewCity(item)"
            >
              {{ item.place_name }}
            </li>
          </template>
        </ul>
      </div>
    </main>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      searchQuery: "",
      queryTimeout: null,
      mapBoxApiKey:
        "pk.eyJ1IjoiZ2xhZGlvIiwiYSI6ImNsZjgwYXp3NjA5bjkzd28yenF4enlqbXkifQ.q1b-8DRWkHNwC53PRjrBrg",
      mapBoxSearchResult: null,
      searchError: null,
    };
  },
  methods: {
    getSearchResult() {
      clearTimeout(this.queryTimeout);
      this.queryTimeout = setTimeout(async () => {
        if (this.searchQuery !== "") {
          try {
            const result =
              await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/
          ${this.searchQuery}.json?access_token=${this.mapBoxApiKey}&types=place`);
            this.mapBoxSearchResult = result.data.features;
          } catch {
            this.searchError = true;
          }
          return;
        }
        this.mapBoxSearchResult = null;
      }, 300);
    },

    previewCity(item) {
      const [city, state] = item.place_name.split(",");
      this.$router.push({
        name: "cityView",
        params: { state: state.replaceAll(" ", ""), city: city },
        query: {
          lat: item.geometry.coordinates[1],
          lng: item.geometry.coordinates[0],
          preview: true,
        },
      });
    },
  },
};
</script>

<style lang="css" scoped>
main {
  padding: 0px 20%;
}
.search:focus {
  outline: none;
  border-bottom: 1px solid #004e71 !important;
  box-shadow: 0px 1px 0 0 #004e71;
}

.search:focus::-webkit-input-placeholder {
  color: white;
}

.result-menu {
  background: #004e71;
  position: absolute;
  padding: 10px;
  margin-top: 10px;
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.result-menu li {
  list-style: none;
  cursor: pointer;
}

@media (max-width: 768px) {
  main {
    padding: 0px;
  }
}
</style>
