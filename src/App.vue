<template>
  <v-app>
    <v-app-bar
      dense
      app
      color="grey lighten-4"
    >
      <div class="d-flex align-center">
        <h1>Guitar.io</h1>
      </div>

      <v-spacer></v-spacer>

      <v-btn
        target="_blank"
        text
      >
        <v-icon>mdi-account</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <Guitar />
    </v-main>
  </v-app>
</template>

<script>
import Guitar from "./components/Guitar";

export default {
  name: "App",

  components: {
    Guitar,
  },

  data: () => ({
    //
  }),
};
</script>
