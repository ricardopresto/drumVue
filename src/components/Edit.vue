<template>
  <div id="container">
    <EditBox
      v-for="beat in currentTrack"
      :key="String(currentTrackNumber) + '.' + String(beat.index)"
      class="edit-box"
      @volume-change="$emit('volume-change', {volume: $event, index: beat.index})"
      @time-change="$emit('time-change', {time: $event, index: beat.index})"
      :volume="beat.volume"
      :timeShifted="beat.timeShifted"
      :class="{ hidden: beat.time == null }"
    />
  </div>
</template>

<script>
import EditBox from "./EditBox";
export default {
  name: "Edit",
  components: {
    EditBox
  },
  props: ["currentTrack", "currentTrackNumber"]
};
</script>

<style scoped>
#container {
  display: flex;
  flex-direction: row;
  width: 840px;
  height: 178px;
  padding: 10px;
  border: 1px solid slateblue;
  border-radius: 10px;
}
.hidden {
  visibility: hidden;
}
</style>