<template>

  <v-container>
    <v-row>
      <v-col cols=6>
        <Frets
          :on=play.on
          :guitar=data
          :loop=switch1
        />
      </v-col>
      <v-col>
        <v-row>Data:</v-row>
        <v-row
          v-for="i in data.frets"
          :key="i"
        >
          <p>{{i}}</p>
        </v-row>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-btn
          @click=OnPlay
          width="15"
          :color=play.color
        >{{play.text}}</v-btn>
        <v-switch
          v-model="switch1"
          label="loop back"
        ></v-switch>

        <v-btn>
          <v-icon>mdi-metronome</v-icon>
        </v-btn>
      </v-col>

    </v-row>

  </v-container>

</template>

<script>
import Frets from "./Frets";
export default {
  data() {
    return {
      data: {
        switch1: false,
        starting: 1,
        frets: [
          { 1: "1", 2: "0", 3: "1", 4: "1", 5: "0", 6: "1" },
          { 1: "0", 2: "0", 3: "0", 4: "0", 5: "0", 6: "0" },
          { 1: "0", 2: "0", 3: "0", 4: "1", 5: "0", 6: "0" },
          { 1: "1", 2: "0", 3: "1", 4: "0", 5: "0", 6: "1" },
          { 1: "1", 2: "0", 3: "0", 4: "0", 5: "1", 6: "0" },
        ],
      },
      play: { on: false, text: "Play", color: "success" },
    };
  },
  methods: {
    OnPlay() {
      if (this.play.on == false) {
        this.play.on = true;
        this.play.text = "Stop";
        this.play.color = "error";
      } else {
        this.play.on = false;
        this.play.color = "success";
        this.play.text = "Play";
      }
    },
  },
  components: { Frets },
};
</script>

<style>
</style>