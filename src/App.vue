<template>
  <div>
    <Transport v-on:play-function="playFunction" v-on:stop-function="stopFunction" />
    <Track v-on:box-click="snareClick" />
    <Track v-on:box-click="kickClick" />
    <Track v-on:box-click="hatClick" />
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
      position: 0
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
        if (elapsed % 10 == 0) {
          vm.position = elapsed / 10 + 1;
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
      this.position = 0;
    },
    snareClick(i) {
      let vm = this;
      if (!vm.snareList.includes(i)) {
        vm.snareList.push(i);
      } else {
        vm.snareList = vm.snareList.filter(n => n != i);
      }
    },
    kickClick(i) {
      let vm = this;
      if (!vm.kickList.includes(i)) {
        vm.kickList.push(i);
      } else {
        vm.kickList = vm.kickList.filter(n => n != i);
      }
    },
    hatClick(i) {
      let vm = this;
      if (!vm.hatList.includes(i)) {
        vm.hatList.push(i);
      } else {
        vm.hatList = vm.hatList.filter(n => n != i);
      }
    }
  }
};
</script>

<style>
</style>
