<template>
  <div id="container">
    <CustomBox
      v-for="beat in currentTrack"
      :key="beat.index"
      class="edit-box"
      @volume-change="$emit('volume-change', {volume: $event, index: beat.index})"
      :volume="beat.volume"
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
  props: ["currentTrack"]
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