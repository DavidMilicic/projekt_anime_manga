<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" sm="12" md="12" lg="12">
        <div class="searchManga">Search for manga:</div>
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
        v-for="manga in mangaSearched"
        :key="manga.mal_id"
        cols="12"
        md="4"
      >
        <div class="image">
          <a :href="manga.url">
            <img :src="manga.image_url" />
          </a>
        </div>

        <manga-cards :manga="manga"></manga-cards>
      </v-col>
    </v-row>
    <v-row>
      <v-col v-if="totalManga > 1" cols="12" sm="12" md="12" lg="12">
        <v-pagination
          :total-visible="7"
          v-model="page"
          :length="Math.ceil(totalManga)"
          circle
        ></v-pagination>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import MangaCards from "../components/MangaCards.vue";
export default {
  components: { MangaCards },

  data() {
    return {
      mangaSearched: [],
      page: 1,
      search: "",
      isLoading: false,
      totalManga: 1,
    };
  },

  methods: {
    getManga() {
      let api =
        "https://api.jikan.moe/v3/search/manga?" +
        "q" +
        "&genre=9,12,33,34&genre_exclude=0";
      this.axios
        .get(api, {
          params: {
            q: this.search,
            page: this.page,
          },
        })
        .then((response) => {
          this.mangaSearched = response.data.results;
          this.totalManga = response.data.last_page;
          console.log(response.data);
          this.isLoading = false;
        });
    },

    fetchEntriesDebounced() {
      clearTimeout(this._searchTimerId);
      this._searchTimerId = setTimeout(() => {
        this.getManga();
      }, 500);
    },
    searchEntries() {
      this.animeSearched = [];
      this.page = 1;
      this.fetchEntriesDebounced();
    },
  },

  watch: {
    page: function () {
      this.getManga();
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
.searchManga {
  text-align: center;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: bold;
  font-size: 40px;
  color: white;
  background: rgba(200, 200, 200, 0.7);
  border-radius: 25px;
}

.image {
  max-height: 350px;
  max-width: 225px;
  object-fit: cover;
  margin-left: auto;
  margin-right: auto;
}
</style>