<template>
  <v-container fluid style="width: 50%">
    <br />
    <v-row>
      <v-col cols="12">
        <v-card height="auto">
          <v-row justify="center"> Log into your account. </v-row>
          <v-form ref="form" v-model="valid" lazy-validation>
            <v-col cols="12">
              <v-text-field
                solo
                outlined
                v-model="username"
                :rules="nameRules"
                label="Username"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                solo
                outlined
                v-model="password"
                :type="show1 ? 'text' : 'password'"
                :rules="passwordRules"
                label="Password"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-btn @click="validate(); submit(); loginAlert = true" :disabled="!valid">Login</v-btn>
            </v-col>
          </v-form>
        </v-card>
        <br />

        <!-- Sa ovim col se malo smanji druga kartica pa bolje izgleda-->

        <v-col>
          <v-card height="auto">
            <v-row justify="center"> Don't have an account? </v-row>
            <v-row justify="center">
              <v-dialog v-model="dialog" persistent max-width="600px">
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    color="light-blue darken-3"
                    dark
                    v-bind="attrs"
                    v-on="on"
                  >
                    Register
                  </v-btn>
                </template>
                <v-card>
                  <v-card-title>
                    <span class="text-h5">User Profile</span>
                  </v-card-title>
                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col cols="12">
                          <v-text-field
                            label="Username*"
                            required
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12">
                          <v-text-field label="Email*" required></v-text-field>
                        </v-col>
                        <v-col cols="12">
                          <v-text-field
                            label="Password*"
                            type="password"
                            required
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6">
                          <v-select
                            :items="['0-17', '18-29', '30-54', '54+']"
                            label="Age*"
                            required
                          ></v-select>
                        </v-col>
                        <v-col cols="12" sm="6">
                          <v-autocomplete
                            :items="[
                              'Shonen',
                              'Shoujo',
                              'Seinen',
                              'Isekai',
                              'Mecha',
                              'Slice of Life',
                              'Romance',
                              'Action',
                              'Sci-fi',
                              'Supernatural',
                              'Post-apocalyptic',
                            ]"
                            label="Interests"
                            multiple
                          ></v-autocomplete>
                        </v-col>
                      </v-row>
                    </v-container>
                    <small>*indicates required field</small>
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="dialog = false">
                      Close
                    </v-btn>
                    <v-btn
                      color="blue darken-1"
                      text
                      @click="
                        dialog = false;
                        registerSuccess = true;
                      "
                    >
                      Save
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-row>
            <v-row justify="center">
              <v-alert
                dense
                type="success"
                dismissible
                v-model="registerSuccess"
                >Succesfully made an account!</v-alert
              >
            </v-row>
          </v-card>
        </v-col>
      </v-col>
    </v-row>
  </v-container>
</template>


<script>
export default {
  data() {
    return {
      show1: false,
      username: "",
      password: "",
      valid: true,
      registerSuccess: false,
      dialog: false,
      loginAlert: false,
      nameRules: [(v) => !!v || "Username is required!"],
      passwordRules: [(v) => !!v || "Password is required!"],
    };
  },

  methods: {
    validate() {
      this.$refs.form.validate();
    },
    submit(){
      if(this.$refs.form.validate()){
        console.log(this.username)
      }
    }
  },
};
</script>
