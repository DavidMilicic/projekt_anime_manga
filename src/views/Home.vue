<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" sm="12" md="12" lg="12">
        <div class="topAnimes">Top 50 animes right now</div>
      </v-col>

      <v-col v-for="anime in animeTop" :key="anime.rank" cols="12" md="4">

        <div class="image">
          <a :href="anime.url">
            <img :src="anime.image_url" />
          </a>
        </div>

        <anime-cards :anime="anime"></anime-cards>

      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import AnimeCards from '../components/AnimeCards.vue';
export default {
  components: { AnimeCards },
  name: "Home",

  data() {
    return {
      animeTop: [],
      totalTopAnimes: 1,
    };
  },

  created() {
    this.getTopAnime();
  },

  methods: {
    getTopAnime() {
      let api = "https://api.jikan.moe/v3/top/anime";
      this.axios
        .get(api)
        .then((response) => {
          console.log(response.data);
          this.animeTop = response.data.top;
          this.totalTopAnimes = response.data.request_cache_expiry;
          this.isLoading = false;
        });
    },
  },

};
</script>

<style lang="scss" scoped>
.topAnimes {
  text-align: center;
  text-transform: uppercase;
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