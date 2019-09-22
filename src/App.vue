<template>
  <div>
    <Transport @play-function="playFunction" @stop-function="stopFunction" />
    <Track @box-click="trackClick($event, track1Array)" @mute-click="trackMute(1)" />
    <Track @box-click="trackClick($event, track2Array)" @mute-click="trackMute(2)" />
    <Track @box-click="trackClick($event, track3Array)" @mute-click="trackMute(3)" />
    <Counter :position="position" />
    <Edit :track1Volume="track1Volume" :track1Array="track1Array" />
  </div>
</template>

<script>
import Transport from "./components/Transport.vue";
import Track from "./components/Track.vue";
import Counter from "./components/Counter.vue";
import Edit from "./components/Edit.vue";

export default {
  name: "App",
  components: {
    Transport,
    Track,
    Counter,
    Edit
  },
  data() {
    return {
      track1Array: [],
      track1Volume: [],
      track2Array: [],
      track3Array: [],
      track1File: new Audio(require("./sounds/snare.mp3")),
      track2File: new Audio(require("./sounds/kick.mp3")),
      track3File: new Audio(require("./sounds/hi-hat.mp3")),
      position: 0,
      mutedTracks: [],
      loop: null,
      speed: 14,
      playing: false
    };
  },
  mounted() {
    for (let n = 0; n < 32; n++) {
      this.track1Array.push(null);
      this.track2Array.push(null);
      this.track3Array.push(null);
      this.track1Volume.push(80);
    }
  },
  methods: {
    playFunction() {
      let elapsed = 0;
      if (!this.playing) {
        this.loop = setInterval(() => {
          if (elapsed % 10 == 0) {
            this.position = elapsed / 10;
          }
          if (this.track1Array.includes(elapsed)) {
            this.track1Play();
          }
          if (this.track2Array.includes(elapsed)) {
            this.track2Play();
          }
          if (this.track3Array.includes(elapsed)) {
            this.track3Play();
          }

          elapsed++;

          if (elapsed == 321) {
            elapsed = 0;
            this.position = 0;
          }
        }, this.speed);
      }
      this.playing = true;
    },

    stopFunction() {
      clearInterval(this.loop);
      this.position = 0;
      this.playing = false;
    },

    track1Play() {
      this.track1File.currentTime = 0;
      this.track1File.volume = this.track1Volume[this.position] / 100;
      this.mutedTracks.includes(1) ? null : this.track1File.play();
    },
    track2Play() {
      this.track2File.currentTime = 0;
      this.mutedTracks.includes(2) ? null : this.track2File.play();
    },
    track3Play() {
      this.track3File.currentTime = 0;
      this.mutedTracks.includes(3) ? null : this.track3File.play();
    },

    trackClick(index, trackArray) {
      if (trackArray[index] == null) {
        trackArray[index] = index * 10;
      } else {
        trackArray[index] = null;
      }
    },

    trackMute(m) {
      if (this.mutedTracks.includes(m)) {
        this.mutedTracks = this.mutedTracks.filter(i => i != m);
      } else {
        this.mutedTracks.push(m);
      }
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
