<template>
  <div>
    <div id="container">
      <div id="trackInfo">
        <div id="name" @click="editClick">{{trackName}}</div>
        <div id="mute" @click="muteClick" :class="{ muted: muted }">
          <svg
            width="20"
            height="20"
            viewBox="0 0 24 24"
            fill="none"
            stroke="#888"
            stroke-width="1"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path d="M11 5L6 9H2v6h4l5 4zM22 9l-6 6M16 9l6 6" />
          </svg>
        </div>
      </div>
      <div
        v-for="beat in trackArray"
        :key="beat.index"
        @click="$emit('box-click', beat.index)"
        class="box"
        :class="{ select: beat.time != null, fourth: beat.index % 4 == 0 }"
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
  width: 160px;
  color: #ddd;
}
.box {
  width: 20px;
  height: 20px;
  margin: 2px;
  display: inline-block;
  flex-shrink: 0;
  border: 1px solid #222;
  border-radius: 3px;
}
#mute {
  margin: 2px 10px;
  height: 20px;
  border: 1px solid #000;
  border-radius: 3px;
}
#name {
  width: 100px;
  font-size: 0.9em;
  cursor: default;
}
.muted {
  background-color: orange;
}
.fourth {
  border: 1px solid #000;
}
.select {
  border: 1px solid slateblue;
  background-color: #54497f;
}
</style>