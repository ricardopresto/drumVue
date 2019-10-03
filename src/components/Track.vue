<template>
  <div>
    <div id="container">
      <div
        v-for="beat in trackArray"
        :key="beat.index"
        @click="$emit('box-click', beat.index)"
        class="box"
        :class="{ select: beat.time != null }"
      ></div>
      <button id="mute" @click="muteClick" :class="{ muted: muted }">Mute</button>
      <button id="edit" @click="editClick">Edit</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Track",
  data() {
    return {
      muted: false
    };
  },
  props: ["trackArray"],
  methods: {
    muteClick() {
      this.$emit("mute-click");
      this.muted = !this.muted;
    },
    editClick() {
      this.$emit("edit-click");
    }
  }
};
</script>

<style scoped>
#container {
  width: 100%;
  height: 20px;
  margin: 10px;
  display: flex;
  flex-direction: row;
  align-content: center;
}
.box {
  width: 20px;
  height: 20px;
  margin: 2px;
  display: inline-block;
  border: 1px solid black;
}
button {
  margin: 2px;
  height: 22px;
}
.muted {
  background-color: red;
}
.editing {
  background-color: slateblue;
}
.select {
  background-color: blue;
}
</style>