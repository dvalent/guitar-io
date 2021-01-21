<template>
  <div class="fretboard">
    <div
      class="fret"
      v-for="(fret, j) in frets"
      :style=fretHeight(j)
      :key=fret
    >
      <div
        class="string"
        v-for="(n, k, i) in fret"
        :key=n
      >
        <div
          v-bind:id="`${i+1}-${j}`"
          v-if="n == 1"
          class="note"
        >
          <div>
            <v-icon color="grey darken-2">mdi-music-note</v-icon>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
// import Note from "~/components/note";
export default {
  // components: { Note },
  props: ["on", "guitar", "loop", "audio", "reset", "starting", "frets"],
  data() {
    return {
      tempo: 200,
      selected: null,
      notesList: [],
      notesListLoop: [],
      playing: null,
    };
  },
  watch: {
    reset() {
      clearInterval(this.playing);
    },
    frets() {
      clearInterval(this.playing);
      this.notesList = [];
      console.log("starting at: ", this.starting);
      this.parseExercise();
    },
    on(d) {
      if (d == true) {
        console.log("Playing is: ", d);
        this.test();
      } else if (d == false) {
        clearInterval(this.playing);
        if (this.selected != null) {
          let activenote = document.getElementById(this.selected);
          activenote.style.background = "grey";
        }
      }
    },
  },
  mounted() {
    clearInterval(this.playing);
    this.notesList = [];
    console.log("starting at: ", this.starting);
    this.parseExercise();
  },
  methods: {
    fretHeight(f) {
      if (f == 0 && this.starting == 0) {
        return `height:35px`;
      } else {
        return `height:70px`;
      }
    },
    playNote(frequency, duration) {
      // create Oscillator node
      var oscillator = this.audio.createOscillator();

      oscillator.type = "square";
      oscillator.frequency.value = frequency; // value in hertz
      oscillator.connect(this.audio.destination);
      oscillator.start();
      setTimeout(function () {
        oscillator.stop();
        this.playNote(frequency, duration);
      }, duration);
    },
    findHz(string, fret) {
      let tuning = {
        1: 82.41,
        2: 110.0,
        3: 146.83,
        4: 196.0,
        5: 246.94,
        6: 329.63,
      };
      console.log("tuning", tuning[string]);
      //https://pages.mtu.edu/~suits/NoteFreqCalcs.html
      return tuning[string] * Math.pow(1.059463094, fret);
    },
    noteColor() {
      return "#E2E2E2";
    },
    async test() {
      if (this.selected != null) {
        let activenote = document.getElementById(this.selected);
        activenote.style.background = "grey";
      }
      let playback = this.notesList;

      if (this.loop == true) {
        console.log("mirroring");
        let mirror = playback.slice();
        mirror.reverse();
        console.log("THIS", mirror);
        mirror.splice(0, 1);
        mirror.forEach((e) => {
          playback.push(e);
        });
      }

      for (let i = 0; i < playback.length; i++) {
        const e = this.notesList[i];
        this.selected = `${e[0]}-${e[1]}`;
        console.log("fret: ", this.selected);
        console.log("CONSTRUCT NOTE", this.findHz(e[0], e[1]));
        this.playNote(this.findHz(e[0], e[1]), this.tempo);
        let activenote = document.getElementById(this.selected);
        activenote.style.background = "yellow";
        await new Promise((r) => {
          this.playing = setTimeout(r, this.tempo);
        });
        //change selected to index - for stop and start
        activenote.style.background = "grey";
        //emit stop
        if (i == this.notesList.length - 1) {
          i = -1;
        }
      }
    },

    parseExercise() {
      for (let i = 1; i < 7; i++) {
        for (let j = 0; j < this.frets.length; j++) {
          const e = this.frets[j];
          if (e[i] == "1") {
            let n = [i.toString(), j.toString()];
            this.notesList.push(n);
          }
        }
      }
      console.log(this.notesList);
    },
  },
  computed: {},
};
</script>

<style scoped>
.fretboard {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
}
.fret {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: space-between;
  background: rgb(226, 226, 226);
  border-top: 1px solid rgb(95, 125, 255);
  width: 100%;
}
.string {
  width: 3px;
  height: 100%;
  background: rgb(95, 125, 255);
  display: flex;
  position: relative;
}
.note {
  top: 50%;
  left: 50%;
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  width: 30px;
  height: 30px;
  border-radius: 50%;
  border: 3px;
  background: grey;
  border-style: solid;
  border-color: rgb(95, 125, 255);
  position: absolute;
}
.note:hover {
  background-color: yellow;
}
</style>