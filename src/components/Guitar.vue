<template>

  <v-container>
    <v-row
      align="center"
      justify="center"
    >
      <v-col
        cols=4
        class=" mt-4"
      >
        <Frets
          :on=play.on
          :guitar=guitardata
          :frets=guitardata.frets
          :loop=switch1
          :audio=audioCtx
          :reset=reset
          :starting="0"
        />
      </v-col>
    </v-row>
    <v-row
      align="center"
      justify="center"
    >
      <v-col
        class="pa-4 mt-4"
        cols=4
      >

        <v-row>

          <v-btn
            @click=OnPlay
            width="15"
            :color=play.color
          >{{play.text}}
          </v-btn>
          <v-spacer></v-spacer>
          <v-btn>
            <v-icon>mdi-refresh</v-icon>
          </v-btn>
          <v-spacer></v-spacer>
          <v-switch
            v-model="switch1"
            label="loop"
          ></v-switch>
          <v-spacer></v-spacer>
          <v-btn @click=Audio>
            <v-icon>mdi-metronome</v-icon>
          </v-btn>

        </v-row>
        <v-row>
          <v-select
            item-text="name"
            item-value="notes"
            :items=exerciseDB
            v-model=guitardata.frets
          ></v-select>
        </v-row>
      </v-col>
    </v-row>

  </v-container>

</template>

<script>
import Frets from "./Frets";
export default {
  data() {
    return {
      audioCtx: null,
      switch1: false,
      reset: false,
      exerciseDB: [
        {
          name: "E Minor Pentatonic",
          notes: [
            { 1: "1", 2: "1", 3: "1", 4: "1", 5: "1", 6: "1" },
            { 1: "0", 2: "0", 3: "0", 4: "0", 5: "0", 6: "0" },
            { 1: "0", 2: "0", 3: "0", 4: "0", 5: "0", 6: "0" },
            { 1: "0", 2: "1", 3: "1", 4: "1", 5: "0", 6: "0" },
            { 1: "1", 2: "0", 3: "0", 4: "0", 5: "1", 6: "1" },
          ],
        },
        {
          name: "C Major Scale",
          notes: [
            { 1: "1", 2: "1", 3: "1", 4: "1", 5: "1", 6: "1" },
            { 1: "1", 2: "0", 3: "0", 4: "0", 5: "1", 6: "1" },
            { 1: "0", 2: "1", 3: "1", 4: "1", 5: "0", 6: "0" },
            { 1: "1", 2: "1", 3: "1", 4: "0", 5: "1", 6: "1" },
          ],
        },
        {
          name: "G Major Scale",
          notes: [
            { 1: "1", 2: "1", 3: "1", 4: "1", 5: "1", 6: "1" },
            { 1: "0", 2: "0", 3: "0", 4: "0", 5: "1", 6: "0" },
            { 1: "1", 2: "1", 3: "1", 4: "1", 5: "0", 6: "1" },
            { 1: "1", 2: "1", 3: "1", 4: "0", 5: "1", 6: "1" },
            { 1: "0", 2: "0", 3: "1", 4: "0", 5: "0", 6: "0" },
          ],
        },
        {
          name: "E Harmonic Minor (Open Position)",
          notes: [
            { 1: "1", 2: "1", 3: "0", 4: "1", 5: "1", 6: "1" },
            { 1: "0", 2: "0", 3: "1", 4: "0", 5: "1", 6: "0" },
            { 1: "1", 2: "1", 3: "1", 4: "1", 5: "0", 6: "1" },
            { 1: "1", 2: "1", 3: "0", 4: "0", 5: "0", 6: "1" },
            { 1: "0", 2: "0", 3: "1", 4: "0", 5: "1", 6: "0" },
          ],
        },
        {
          name: "Dorian",
          notes: [
            { 1: "1", 2: "1", 3: "1", 4: "1", 5: "0", 6: "0" },
            { 1: "0", 2: "0", 3: "0", 4: "0", 5: "0", 6: "0" },
            { 1: "1", 2: "1", 3: "1", 4: "1", 5: "1", 6: "1" },
            { 1: "1", 2: "0", 3: "0", 4: "0", 5: "1", 6: "1" },
            { 1: "0", 2: "0", 3: "1", 4: "1", 5: "1", 6: "0" },
            { 1: "0", 2: "0", 3: "0", 4: "0", 5: "1", 6: "1" },
          ],
        },
      ],
      guitardata: {
        tempo: 100,
        starting: null,
        frets: [
          { 1: "1", 2: "1", 3: "1", 4: "1", 5: "1", 6: "1" },
          { 1: "0", 2: "0", 3: "0", 4: "0", 5: "0", 6: "0" },
          { 1: "0", 2: "0", 3: "0", 4: "0", 5: "0", 6: "0" },
          { 1: "0", 2: "1", 3: "1", 4: "1", 5: "0", 6: "0" },
          { 1: "1", 2: "0", 3: "0", 4: "0", 5: "1", 6: "1" },
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
    this.starting = 0;
    this.audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  },
  components: { Frets },
};
</script>

<style>
</style>