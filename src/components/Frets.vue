<template>
  <div class="fretboard">
    <div
      class="fret"
      v-for="(fret, j) in guitar.frets"
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
          <p>
            <v-icon color="grey darken-2">mdi-music-note</v-icon>
          </p>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
// import Note from "~/components/note";
export default {
  // components: { Note },
  props: ["on", "guitar", "loop"],
  data() {
    return {
      selected: null,
      notesList: [],
      notesListLoop: [],
      playing: null,
    };
  },
  watch: {
    on(d) {
      if (d == true) {
        console.log("changed to: ", d);
        this.test();
      } else if (d == false) {
        clearInterval(this.playing);
      }
    },
  },
  mounted() {
    this.parseExercise();
  },
  methods: {
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
        let activenote = document.getElementById(this.selected);
        activenote.style.background = "yellow";
        await new Promise((r) => {
          this.playing = setTimeout(r, 200);
        });
        //change selected to index - for stop and start
        activenote.style.background = "grey";
        //emit stop
        if (i == this.notesList.length - 1) {
          i = 0;
        }
      }
    },

    parseExercise() {
      for (let i = 1; i < 7; i++) {
        for (let j = 0; j < this.guitar.frets.length; j++) {
          const e = this.guitar.frets[j];
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
  height: 70px;
  width: 250px;
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