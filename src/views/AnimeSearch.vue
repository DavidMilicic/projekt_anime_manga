<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" sm="12" md="12" lg="12">
        <!-- Razdvaja ova 3 retka -->
        <div class="searchAnime">Search for anime:</div>
      </v-col>

      <!-- Search box -->

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

      <v-col v-for="anime in animeSearched" :key="anime.mal_id" cols="12" md="4">
        <!-- Da prikaze anime po iID -->

        <!-- Slika za svaki anime -->
        <div class="image">
          <a :href="anime.url">
            <img :src="anime.image_url" />
          </a>
        </div>

        <!-- Opis za svaki anime -->

        <v-card color="grey" height="auto">
          <div class="card">{{ anime.title }}</div>
          <div class="cardText">
            Number of episodes: {{ anime.episodes }} <br />
            Aired from: {{ anime.start_date }} to {{ anime.end_date }} <br />
            Type: {{ anime.type }} <br />
            Score: {{ anime.score }} <br />
            Description: {{ anime.synopsis }}
          </div>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "Home",

  data() {
    return {
      animeSearched: [],
      page: 1,
      perPage: 50,
      search: "",
      isLoading: false,
    };
  },

  methods: {

    getAnimes() {
      let api = "https://api.jikan.moe/v3/search/anime?" + 'q' + "&page=1&genre=9,12,33,34&genre_exclude=0"; //da filtera NSFW
      this.axios
        .get(api, {
          params: {
            'q': this.search
          },
        })
        .then((response) => {
          this.animeSearched = response.data.results;
          console.log(response.data);
          this.isLoading = false;
        });
    },

    fetchEntriesDebounced() {
      clearTimeout(this._searchTimerId);
      this._searchTimerId = setTimeout(() => {
        this.getAnimes();
      }, 500);
    },
  },

  watch: {
    search(val) {
      if (!val) {
        return;
      }
      this.isLoading = true;
      this.fetchEntriesDebounced();
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

.card {
  text-align: center;
  font-size: 22px;
  font-weight: bold;
}

.cardText {
  font-size: 20px;
}
</style>