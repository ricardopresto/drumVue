<template>
  <div id="container">
    <EditBox class="edit-box" v-for="i in 33" :key="i" @volume-change="volChange($event, i)" />
  </div>
</template>

<script>
import EditBox from "./EditBox.vue";
export default {
  name: "Edit",
  components: {
    EditBox
  },
  props: ["track1Volume"],
  methods: {
    volChange(volume, index) {
      this.track1Volume[index - 1] = Number(volume);
      console.log(this.track1Volume);
    }
  }
};
</script>

<style scoped>
#container {
  display: flex;
  flex-direction: row;
  width: 100%;
}
</style>