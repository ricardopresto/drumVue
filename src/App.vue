<template>
  <div>
    <Transport @play-function="playFunction" @stop-function="stopFunction" />
    <Track @box-click="trackClick($event, track1List)" @mute-click="track1Mute" />
    <Track @box-click="trackClick($event, track2List)" @mute-click="track2Mute" />
    <Track @box-click="trackClick($event, track3List)" @mute-click="track3Mute" />
    <Counter :position="position" />
  </div>
</template>

<script>
import Transport from "./components/Transport.vue";
import Track from "./components/Track.vue";
import Counter from "./components/Counter.vue";

export default {
  name: "App",
  components: {
    Transport,
    Track,
    Counter
  },
  data() {
    return {
      track1List: [],
      track2List: [],
      track3List: [],
      track1File: new Audio(require("./sounds/snare.mp3")),
      track2File: new Audio(require("./sounds/kick.mp3")),
      track3File: new Audio(require("./sounds/hi-hat.mp3")),
      position: 0,
      track1Muted: false,
      track2Muted: false,
      track3Muted: false,
      loop: null
    };
  },
  mounted() {
    for (let n = 0; n < 32; n++) {
      this.track1List.push(null);
      this.track2List.push(null);
      this.track3List.push(null);
    }
  },
  methods: {
    playFunction() {
      let elapsed = 0;
      this.loop = setInterval(() => {
        if (this.track1List.includes(elapsed)) {
          this.track1Play();
        }
        if (this.track2List.includes(elapsed)) {
          this.track2Play();
        }
        if (this.track3List.includes(elapsed)) {
          this.track3Play();
        }
        if (elapsed % 10 == 0) {
          this.position = elapsed / 10 + 1;
        }
        elapsed = elapsed + 1;
        if (elapsed == 321) {
          elapsed = 0;
        }
      }, 14);
    },

    stopFunction() {
      clearInterval(this.loop);
      this.position = 0;
    },

    track1Play() {
      this.track1File.currentTime = 0;
      this.track1Muted ? null : this.track1File.play();
    },
    track2Play() {
      this.track2File.currentTime = 0;
      this.track2Muted ? null : this.track2File.play();
    },
    track3Play() {
      this.track3File.currentTime = 0;
      this.track3Muted ? null : this.track3File.play();
    },

    trackClick(index, trackArray) {
      if (trackArray[index] == null) {
        trackArray[index] = index * 10;
      } else {
        trackArray[index] = null;
      }
    },

    track1Mute() {
      this.track1Muted = !this.track1Muted;
    },
    track2Mute() {
      this.track2Muted = !this.track2Muted;
    },
    track3Mute() {
      this.track3Muted = !this.track3Muted;
    }
  }
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
}
</style>
