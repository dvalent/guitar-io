<template>
  <v-app>
    <v-app-bar
      dense
      app
      color="white"
      dark
    >
      <div class="d-flex align-center">
        <h2>Guitar-io</h2>
      </div>

      <v-spacer></v-spacer>

      <v-btn
        target="_blank"
        text
      >
        <v-icon color=black>mdi-account-circle-outline</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <Guitar />
    </v-main>
  </v-app>
</template>

<script>
// import HelloWorld from "./components/HelloWorld";
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
