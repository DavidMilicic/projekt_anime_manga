<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" sm="12" md="12" lg="12">
        <div class="topAnimes">Top 50 animes right now</div>
      </v-col>

      <v-col v-for="anime in animeTop" :key="anime.rank" cols="12" md="4">
        <!-- Da prikaze top anime po ranku -->

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
            Score: {{ anime.score }}
          </div>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-pagination
        v-model="page"
        :length="totalAnimes / perPage"
        circle
      ></v-pagination>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "Home",

  data() {
    return {
      animeTop: [],
      page: 1,
      totalTopAnimes: 0,
      perPage: 50,
    };
  },

  created() {
    this.getTopAnime();
  },

  methods: {
    getTopAnime() {
      let api = "https://api.jikan.moe/v3/top/anime";
      this.axios
        .get(api, {
          params: {
            offset: this.perPage * (this.page - 1),
          },
        })
        .then((response) => {
          console.log(response.data);
          this.animeTop = response.data.top;
          this.totalTopAnimes = response.data.request_cache_expiry;
          this.isLoading = false;
        });
    },
  },

  watch: {
    page: function () {
      this.getTopAnime();
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

.card {
  text-align: center;
  font-size: 22px;
  font-weight: bold;
}

.cardText {
  font-size: 20px;
}
</style>