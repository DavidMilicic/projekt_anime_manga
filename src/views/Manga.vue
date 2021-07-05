<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" sm="12" md="12" lg="12">
        <!-- Razdvaja ova 3 retka -->
        <div class="searchManga">Search for manga:</div>
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

      <v-col v-for="manga in mangaSearched" :key="manga.mal_id" cols="12" md="4">
        <!-- Da prikaze mange po iID -->

        <!-- Slika za svaku mangu -->
        <div class="image">
          <a :href="manga.url">
            <img :src="manga.image_url" />
          </a>
        </div>

        <!-- Opis za svaku mangu -->

        <v-card color="grey" height="auto">
          <div class="card">{{ manga.title }}</div>
          <div class="cardText">
            Number of episodes: {{ manga.episodes }} <br />
            Started: {{ manga.start_date }} | Ended: {{ manga.end_date }} <br />
            Type: {{ manga.type }} <br />
            Score: {{ manga.score }} <br />
            Description: {{ manga.synopsis }}
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
      mangaSearched: [],
      page: 1,
      perPage: 50,
      search: "",
      isLoading: false,
    };
  },

  methods: {

    getManga() {
      let api = "https://api.jikan.moe/v3/search/manga?" + 'q' + "&page=1&genre=9,12,33,34&genre_exclude=0"; //da filtera NSFW
      this.axios
        .get(api, {
          params: {
            'q': this.search
          },
        })
        .then((response) => {
          this.mangaSearched = response.data.results;
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

.card {
  text-align: center;
  font-size: 22px;
  font-weight: bold;
}

.cardText {
  font-size: 20px;
}
</style>