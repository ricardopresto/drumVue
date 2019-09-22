<template>
  <div id="container">
    <div v-for="i in 32" :key="i">
      <CustomBox
        class="edit-box"
        @volume-change="volChange($event, i)"
        :initialVol="track1Volume[i - 1]"
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
  props: ["track1Volume", "currentTrack"],
  methods: {
    volChange(volume, index) {
      this.track1Volume[index - 1] = Number(volume);
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