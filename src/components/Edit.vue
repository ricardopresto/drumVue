<template>
  <div id="container">
    <div v-for="i in 32" :key="i">
      <CustomBox
        class="edit-box"
        @volume-change="volChange($event, i)"
        :initialVol="volumeArrays[currentTrack - 1][i - 1]"
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
  props: ["volumeArrays", "currentTrack"],
  watch: {
    currentTrack() {
      console.log("Current track", this.currentTrack);
    }
  },
  methods: {
    volChange(volume, index) {
      this.volumeArrays[this.currentTrack - 1][index - 1] = Number(volume);
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