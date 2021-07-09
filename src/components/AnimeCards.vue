<template>
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
</template>

<script>
export default {
  props: ["anime"]
}

</script>

<style lang="scss" scoped>
.card {
  text-align: center;
  font-size: 22px;
  font-weight: bold;
}

.cardText {
  font-size: 20px;
}
</style>