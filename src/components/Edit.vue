<template>
  <div id="container">
    <div v-for="beat in trackArrays[currentTrack]" :key="beat.index">
      <CustomBox
        class="edit-box"
        @volume-change="volChange($event, beat.index)"
        :initialVol="beat.volume"
      />
    </div>
  </div>
</template>

<script>
import CustomBox from "./CustomBox";
export default {
  name: "Edit",
  components: {
    CustomBox
  },
  props: ["currentTrack", "trackArrays"],
  watch: {
    currentTrack() {
      console.log("Current track", this.currentTrack);
    }
  },
  methods: {
    volChange(volume, index) {
      this.trackArrays[this.currentTrack].forEach(beat => {
        if (beat.index == index) {
          beat.volume = volume;
        }
      });
    }
  }
};
</script>

<style scoped>
#container {
  display: flex;
  flex-direction: row;
  width: 100%;
  margin: 10px;
}
</style>