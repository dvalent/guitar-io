<template>

  <v-container>
    <v-row>
      <v-col cols=6>
        <Frets
          :on=play.on
          :guitar=data
          :loop=switch1
          :audioCtx=audioCtx
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

      <v-btn
        @click=OnPlay
        width="15"
        :color=play.color
      >{{play.text}}</v-btn>
      <v-btn>
        <v-icon>mdi-refresh</v-icon>
      </v-btn>
      <v-switch
        v-model="switch1"
        label="loop back"
      ></v-switch>

      <v-btn @click=Audio>
        <v-icon>mdi-metronome</v-icon>
      </v-btn>

    </v-row>

  </v-container>

</template>

<script>
import Frets from "./Frets";
export default {
  data() {
    return {
      data: {
        tempo: 100,
        audioCtx: null,
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
    Audio() {
      // create web audio api context

      let playNote = (frequency, duration) => {
        // create Oscillator node
        var oscillator = this.audioCtx.createOscillator();

        oscillator.type = "square";
        oscillator.frequency.value = frequency; // value in hertz
        oscillator.connect(this.audioCtx.destination);
        oscillator.start();

        setTimeout(function () {
          oscillator.stop();
          playMelody();
        }, duration);
      };

      function playMelody() {
        if (notes.length > 0) {
          let note = notes.pop();
          playNote(note[0], (1000 * 256) / (note[1] * tempo));
        }
      }

      let notes = [
        [659, 4],
        [659, 4],
        [659, 4],
        [523, 8],
        [0, 16],
        [783, 16],
        [659, 4],
        [523, 8],
        [0, 16],
        [783, 16],
        [659, 4],
        [0, 4],
        [987, 4],
        [987, 4],
        [987, 4],
        [1046, 8],
        [0, 16],
        [783, 16],
        [622, 4],
        [523, 8],
        [0, 16],
        [783, 16],
        [659, 4],
      ];

      notes.reverse();
      let tempo = 100;

      playMelody();
    },
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
  mounted() {
    this.audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  },
  components: { Frets },
};
</script>

<style>
</style>