<template>
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
</template>

<script>
export default {
  props: ["manga"]
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