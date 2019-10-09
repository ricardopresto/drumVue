<template>
  <div>
    <div id="container">
      <button @click="$emit('play-function')">
        <svg
          width="30"
          height="30"
          viewBox="0 0 24 24"
          fill="none"
          stroke="#000000"
          stroke-width="1"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <polygon points="5 3 19 12 5 21 5 3" />
        </svg>
      </button>
      <button @click="$emit('pause-function')" :class="{paused: paused}">
        <svg
          width="30"
          height="30"
          viewBox="0 0 24 24"
          fill="none"
          stroke="#000000"
          stroke-width="1"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <rect x="6" y="4" width="4" height="16" />
          <rect x="14" y="4" width="4" height="16" />
        </svg>
      </button>
      <button @click="$emit('stop-function')">
        <svg
          width="30"
          height="30"
          viewBox="0 0 24 24"
          fill="none"
          stroke="#000000"
          stroke-width="1"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <rect x="4" y="4" width="16" height="16" />
        </svg>
      </button>
      <button @click="$emit('reset-function')">
        <svg
          width="30"
          height="30"
          viewBox="0 0 24 24"
          fill="none"
          stroke="#000000"
          stroke-width="1"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <path d="M2.5 2v6h6M2.66 15.57a10 10 0 1 0 .57-8.38" />
        </svg>
      </button>
      <input v-model="speedSlider" type="range" min="6" max="30" value="22" @change="speedChange" />
      <div>
        <span @click="$emit('length-change', 16)">16&ensp;</span>
        <span @click="$emit('length-change', 32)">32</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Controls",
  data: function() {
    return {
      speedSlider: ""
    };
  },
  props: ["paused"],
  methods: {
    speedChange() {
      this.$emit("speed-change", this.speedSlider);
    }
  }
};
</script>

<style scoped>
#container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-left: 20px;
  padding: 20px;
  border: 1px solid slateblue;
  border-radius: 10px;
}
button {
  width: 100px;
  height: 40px;
  margin: 5px;
}
.paused {
  background-color: lightslategrey;
}
input {
  width: 100px;
  margin: 5px;
}
</style>
