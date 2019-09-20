<template>
  <div>
    <Transport v-on:play-function="playFunction" v-on:stop-function="stopFunction" />
    <Track v-on:box-click="trackClick($event, snareList)" v-on:mute-click="snareMute" />
    <Track v-on:box-click="trackClick($event, kickList)" v-on:mute-click="kickMute" />
    <Track v-on:box-click="trackClick($event, hatList)" v-on:mute-click="hatMute" />
    <Counter v-bind:position="position" />
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
      snareList: [],
      kickList: [],
      hatList: [],
      snare: new Audio(require("./sounds/snare.mp3")),
      kick: new Audio(require("./sounds/kick.mp3")),
      hat: new Audio(require("./sounds/hi-hat.mp3")),
      position: 0,
      snareMuted: false,
      kickMuted: false,
      hatMuted: false,
      loop: null
    };
  },
  mounted() {
    for (let n = 0; n < 32; n++) {
      this.snareList.push(null);
      this.kickList.push(null);
      this.hatList.push(null);
    }
  },
  methods: {
    playFunction() {
      let elapsed = 0;
      this.loop = setInterval(() => {
        if (this.snareList.includes(elapsed)) {
          this.snarePlay();
        }
        if (this.kickList.includes(elapsed)) {
          this.kickPlay();
        }
        if (this.hatList.includes(elapsed)) {
          this.hatPlay();
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

    hatPlay() {
      this.hat.currentTime = 0;
      this.hatMuted ? null : this.hat.play();
    },
    snarePlay() {
      this.snare.currentTime = 0;
      this.snareMuted ? null : this.snare.play();
    },
    kickPlay() {
      this.kick.currentTime = 0;
      this.kickMuted ? null : this.kick.play();
    },

    trackClick(index, trackArray) {
      if (trackArray[index] == null) {
        trackArray[index] = index * 10;
      } else {
        trackArray[index] = null;
      }
    },

    snareMute() {
      this.snareMuted = !this.snareMuted;
    },
    kickMute() {
      this.kickMuted = !this.kickMuted;
    },
    hatMute() {
      this.hatMuted = !this.hatMuted;
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
