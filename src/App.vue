<template>
  <div>
    <Transport v-on:play-function="playFunction" v-on:stop-function="stopFunction" />
    <Track v-on:box-click="snareClick" />
    <Track v-on:box-click="kickClick" />
    <Track v-on:box-click="hatClick" />
  </div>
</template>

<script>
import Transport from "./components/Transport.vue";
import Track from "./components/Track.vue";

export default {
  name: "App",
  components: {
    Transport,
    Track
  },
  data() {
    return {
      snareList: [],
      kickList: [],
      hatList: [],
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
        if (elapsed == 321) {
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
    },
    snareClick(i) {
      let vm = this;
      let index = i * 10 - 10;
      if (!vm.snareList.includes(index)) {
        vm.snareList.push(index);
      } else {
        vm.snareList = vm.snareList.filter(n => n != index);
      }
    },
    kickClick(i) {
      let vm = this;
      let index = i * 10 - 10;
      if (!vm.kickList.includes(index)) {
        vm.kickList.push(index);
      } else {
        vm.kickList = vm.kickList.filter(n => n != index);
      }
    },
    hatClick(i) {
      let vm = this;
      let index = i * 10 - 10;
      if (!vm.hatList.includes(index)) {
        vm.hatList.push(index);
      } else {
        vm.hatList = vm.hatList.filter(n => n != index);
      }
    }
  }
};
</script>

<style>
</style>
