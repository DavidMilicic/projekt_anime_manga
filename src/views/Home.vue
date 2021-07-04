<template>
  <v-container>
    <v-row>

      <v-col cols="1" sm="12" md="12"> <!-- Razdvaja ova 3 retka -->
      <div class="searchAnime">Search for anime:</div>
      </v-col>

      <!-- Search box -->

      <v-col cols="1" sm="12" md="12">
        <v-text-field
          dark
          label="Type here..."
          placeholder="Bleach, Naruto, Dragon Ball, etc."
          filled
        ></v-text-field>
      </v-col>

      <v-col cols="1" sm="12" md="12">
      <div class="topAnimes">Top animes right now</div>
      </v-col>
      
      <v-col v-for="anime in animeTop" :key="anime.rank" cols="4"> <!-- Da prikaze top anime po ranku -->

      <!-- Slika za svaki anime -->
        <v-img
          max-height="318"
          max-width="225"
          class="mx-auto"
          :src="anime.image_url"
        ></v-img>

        <!-- Opis za svaki anime -->

        <v-card color="grey" height="200">
          <div class="card">{{ anime.title }}</div>
          <div class="cardText"> Number of episodes: {{ anime.episodes }} <br/> Aired from: {{ anime.start_date }} to {{ anime.end_date }} <br/> Type: {{ anime.type }} <br/> Score: {{ anime.score }} </div>
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
      animeTop: [],
    };
  },

  created() {
    this.getTopAnime();
  },

  methods: {
    getTopAnime() {
      let api = "https://api.jikan.moe/v3/top/anime";
      this.axios.get(api).then((response) => {
        console.log(response.data);
        this.animeTop = response.data.top;
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

.searchAnime {
  text-align: center;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: bold;
  font-size: 40px;
  color: white;
  background: rgba(200, 200, 200, 0.7);
  border-radius: 25px;
  
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