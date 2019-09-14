<template>
  <div>
    <Transport v-on:play-function="playFunction" v-on:stop-function="stopFunction" />
  </div>
</template>

<script>
import Transport from "./components/Transport.vue";

export default {
  name: "App",
  components: {
    Transport
  },
  data() {
    return {
      snareList: [40, 70, 120, 200, 280, 310],
      kickList: [0, 100, 160, 260],
      hatList: [
        0,
        20,
        40,
        60,
        80,
        100,
        120,
        140,
        160,
        180,
        200,
        220,
        240,
        260,
        280,
        300
      ],
      snare: new Audio(require("./sounds/snare.mp3")),
      kick: new Audio(require("./sounds/kick.mp3")),
      hat: new Audio(require("./sounds/hat.mp3"))
    };
  },
  methods: {
    playFunction() {
      let elapsed = 0;
      let vm = this;
      window.loop = setInterval(function() {
        if (vm.snareList.includes(elapsed)) {
          vm.snarePlay();
        }
        if (vm.kickList.includes(elapsed)) {
          vm.kickPlay();
        }
        if (vm.hatList.includes(elapsed)) {
          vm.hatPlay();
        }

        elapsed = elapsed + 1;
        if (elapsed == 320) {
          elapsed = 0;
        }
      }, 14);
    },
    hatPlay() {
      this.hat.currentTime = 0;
      this.hat.play();
    },
    snarePlay() {
      this.snare.currentTime = 0;
      this.snare.play();
    },
    kickPlay() {
      this.kick.currentTime = 0;
      this.kick.play();
    },
    stopFunction() {
      clearInterval(loop);
    }
  }
};
</script>

<style>
</style>
