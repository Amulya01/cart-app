<template>
  <div id="app">
    <v-app>
      <main>
        <v-container fluid fill-height class="loginOverlay">
          <v-layout flex align-center justify-center>
            <v-flex xs12 sm4 elevation-6>
              <v-toolbar class="pt-5 blue darken-1">
                <v-toolbar-title class="white--text"
                  ><h4>Welcome Back</h4></v-toolbar-title
                >
              </v-toolbar>
              <v-card>
                <v-card-text class="pt-4">
                  <div>
                    <v-form v-model="valid" ref="form">
                      <v-text-field
                        label="Enter your e-mail address"
                        v-model="email"
                        :rules="emailRules"
                        required
                      ></v-text-field>
                      <v-text-field
                        label="Enter your password"
                        v-model="password"
                        min="8"
                        :append-icon="e1 ? 'visibility' : 'visibility_off'"
                        :append-icon-cb="() => (e1 = !e1)"
                        :type="e1 ? 'password' : 'text'"
                        :rules="passwordRules"
                        counter
                        required
                      ></v-text-field>
                      <v-layout justify-space-between>
                        <v-btn
                          @click="submit"
                          :class="{
                            'blue darken-4 white--text': valid,
                            disabled: !valid
                          }"
                          >Login</v-btn
                        >
                        <a href="">Forgot Password</a>
                      </v-layout>
                    </v-form>
                  </div>
                </v-card-text>
              </v-card>
            </v-flex>
          </v-layout>
        </v-container>

        <a
          href="https://www.beautycolorcode.com/87cefa-2880x1800.png"
          target="_blank"
        >
        </a>
      </main>
    </v-app>
  </div>
</template>
<script>
export default {
  data() {
    return {
      valid: false,
      e1: false,
      password: "",
      passwordRules: [v => !!v || "Password is required"],
      email: "",
      emailRules: [
        v => !!v || "E-mail is required",
        v =>
          /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(v) ||
          "E-mail must be valid"
      ]
    };
  },
  methods: {
    submit() {
      this.$store.dispatch("userSignIn", {
        email: this.email,
        password: this.password
      });
      console.log(this.$store.getters.isAuthenticated);
    },
    clear() {
      this.$refs.form.reset();
    }
  }
};
</script>
<style scoped>
#app {
  background-image: url("");
  background-size: cover;
  overflow: hidden;
}

.loginOverlay {
  background: rgba(33, 150, 243, 0.3);
}

.photoCredit {
  position: absolute;
  bottom: 15px;
  right: 15px;
}
</style>
