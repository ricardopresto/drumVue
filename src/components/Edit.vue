<template>
  <div id="container">
    <CustomBox
      v-for="beat in currentTrack"
      :key="String(currentTrackNumber) + '.' + String(beat.index)"
      class="edit-box"
      @volume-change="$emit('volume-change', {volume: $event, index: beat.index})"
      :volume="beat.volume"
      :class="{ hidden: beat.time == null }"
    />
  </div>
</template>

<script>
import CustomBox from "./CustomBox";
export default {
  name: "Edit",
  components: {
    CustomBox
  },
  props: ["currentTrack", "currentTrackNumber"]
};
</script>

<style scoped>
#container {
  display: flex;
  flex-direction: row;
  width: 100%;
  margin: 10px;
}
.hidden {
  visibility: hidden;
}
</style>