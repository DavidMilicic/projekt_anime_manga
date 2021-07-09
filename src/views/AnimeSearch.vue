<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" sm="12" md="12" lg="12">
        <div class="searchAnime">Search for anime:</div>
      </v-col>

      <v-col cols="12" sm="12" md="12" lg="12">
        <v-text-field
          append-icon="mdi-magnify"
          dark
          filled
          label="3 letters minimum!"
          placeholder="Bleach, Naruto, Dragon Ball, etc."
          v-model="search"
          :loading="isLoading"
          @submit.prevent="search"
          clearable
        ></v-text-field>
      </v-col>

      <v-col
        v-for="anime in animeSearched"
        :key="anime.mal_id"
        cols="12"
        md="4"
      >
        <div class="image">
          <a :href="anime.url">
            <img :src="anime.image_url" />
          </a>
        </div>

        <anime-cards :anime="anime"></anime-cards>
      </v-col>
    </v-row>
    <v-row>
      <v-col v-if="totalAnimes > 1 " cols="12" sm="12" md="12" lg="12">
      <v-pagination
        :total-visible="7"
        v-model="page"
        :length="Math.ceil(totalAnimes)"
        circle
      ></v-pagination>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import AnimeCards from '../components/AnimeCards.vue'

export default {
  components: { AnimeCards },
  name: "Home",

  data() {
    return {
      animeSearched: [],
      page: 1,
      search: "",
      isLoading: false,
      totalAnimes: 1,
    };
  },

  methods: {

    getAnimes() {
      let api = "https://api.jikan.moe/v3/search/anime?" + "&genre=9,12,33,34&genre_exclude=0";
      this.axios
        .get(api, {
          params: {
            "q": this.search,
            "page": this.page,
            
          },
        })
        .then((response) => {
          this.animeSearched = response.data.results;
          this.totalAnimes = response.data.last_page;
          console.log(response.data);
          this.isLoading = false;
        });
    },

    fetchEntriesDebounced() {
      clearTimeout(this._searchTimerId);
      this._searchTimerId = setTimeout(() => {
        this.getAnimes();
      }, 750);
    },

    searchEntries(){
      this.animeSearched = []
      this.page = 1
      this.fetchEntriesDebounced()
    }
  },

  watch: {

    page: function () {
      this.getAnimes();
      window.scrollTo(0, 0);
    },

    search(val) {
      if (!val) {
        return;
      }
      this.isLoading = true;
      this.searchEntries();
    },
  },
};
</script>

<style lang="scss" scoped>
.searchAnime {
  text-align: center;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: bold;
  font-size: 40px;
  color: white;
  background: rgba(200, 200, 200, 0.7);
  border-radius: 25px;
}

.image {
  max-height: 318px;
  max-width: 225px;
  margin-left: auto;
  margin-right: auto;
}

</style>