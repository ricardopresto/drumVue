<template>
  <div>
    <div id="container">
      <div id="trackInfo">
        <div id="name" @click="editClick">{{trackName}}</div>
        <button id="mute" @click="muteClick" :class="{ muted: muted }">
          <svg
            width="20"
            height="20"
            viewBox="0 0 24 24"
            fill="none"
            stroke="#000000"
            stroke-width="1"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path d="M11 5L6 9H2v6h4l5 4zM22 9l-6 6M16 9l6 6" />
          </svg>
        </button>
      </div>
      <div
        v-for="beat in trackArray"
        :key="beat.index"
        @click="$emit('box-click', beat.index)"
        class="box"
        :class="{ select: beat.time != null }"
      ></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Track",
  data() {
    return {
      muted: false,
      isCurrentTrack: false
    };
  },
  props: ["trackArray", "trackName"],
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
  height: 30px;
  margin: 5px 10px;
  display: flex;
  flex-direction: row;
  align-items: center;
}
#trackInfo {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  align-items: center;
  flex-shrink: 0;
  width: 130px;
}
.box {
  width: 20px;
  height: 20px;
  margin: 2px;
  display: inline-block;
  flex-shrink: 0;
  border: 1px solid black;
}
button {
  margin: 2px;
  height: 22px;
}
#name {
  width: 100px;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 0.9em;
  cursor: default;
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