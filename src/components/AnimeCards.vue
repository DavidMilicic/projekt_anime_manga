<template>
  <v-row>
    <v-col>
      <v-card color="grey" height="auto">
        <div class="card">{{ anime.title }}</div>
        <div class="cardText">
          Number of episodes: {{ anime.episodes }} <br />
          Aired from: {{ anime.start_date }} to {{ anime.end_date }} <br />
          Type: {{ anime.type }} <br />
          Score: {{ anime.score }}
        </div>

    <v-alert dense type="success" dismissible v-model="successAlert">
      Successfully added to favourites!</v-alert
    >

    <v-alert dense type="error" color="black" dismissible v-model="failedAlert">
      Seems like you've changed your mind!</v-alert
    >
    
        <!-- Dialog for more info -->

        <div class="dialog">
          <v-dialog v-model="dialog" width="500">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="light-blue darken-3" dark v-bind="attrs" v-on="on">
                More info
              </v-btn>
            </template>

            <v-card>
              <v-card-title class="text-h5 grey lighten-2">
                Synopsis:
              </v-card-title>

              <v-card-text>
                {{ anime.synopsis }}
              </v-card-text>
              <v-col> Want to know more? Click on the button below! </v-col>
              <v-divider></v-divider>

              <v-card-actions>
                <v-btn
                  color="primary"
                  text
                  @click="dialog = false"
                  :href="anime.url"
                  target="_blank"
                >
                  Click me!
                </v-btn>
                <v-spacer></v-spacer>
                <v-btn color="red" text @click="dialog = false"> Close. </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </div>

        <!-- Dialog for favourite -->

        <div class="dialog">
          <v-dialog v-model="favourite" width="500">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="amber darken-3" dark v-bind="attrs" v-on="on">
                Favourite
              </v-btn>
            </template>
            <v-card>
              <v-card-title class="text-h5 grey lighten-2">
                Favourite?
              </v-card-title>

              <v-card-text>
                Are you sure you want to add this anime to your favourites list?
              </v-card-text>

              <v-divider></v-divider>

              <v-card-actions>
                <v-btn
                  color="green"
                  text
                  @click="
                    favourite = false;
                    successAlert = true;
                  "
                >
                  Yes, I do.
                </v-btn>
                <v-spacer></v-spacer>
                <v-btn
                  color="red"
                  text
                  @click="
                    favourite = false;
                    failedAlert = true;
                  "
                >
                  No, I don't.
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </div>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      dialog: false,
      favourite: false,
      successAlert: false,
      failedAlert: false,
    };
  },
  props: ["anime"],
};
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

.dialog {
  text-align: center;
}
</style>