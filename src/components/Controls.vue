<template>
  <div>
    <div id="container">
      <button @click="$emit('play-function')">
        <svg
          width="30"
          height="30"
          viewBox="0 0 24 24"
          fill="none"
          stroke="#ddd"
          stroke-width="1"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <polygon points="5 3 19 12 5 21 5 3" />
        </svg>
        <span>Play</span>
      </button>
      <button @click="$emit('pause-function')" :class="{paused: paused}">
        <svg
          width="30"
          height="30"
          viewBox="0 0 24 24"
          fill="none"
          stroke="#ddd"
          stroke-width="1"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <rect x="6" y="4" width="4" height="16" />
          <rect x="14" y="4" width="4" height="16" />
        </svg>
        <span>Pause</span>
      </button>
      <button @click="$emit('stop-function')">
        <svg
          width="30"
          height="30"
          viewBox="0 0 24 24"
          fill="none"
          stroke="#ddd"
          stroke-width="1"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <rect x="4" y="4" width="16" height="16" />
        </svg>
        <span>Stop</span>
      </button>
      <button @click="$emit('reset-function')">
        <svg
          width="30"
          height="30"
          viewBox="0 0 24 24"
          fill="none"
          stroke="#ddd"
          stroke-width="1"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <path d="M2.5 2v6h6M2.66 15.57a10 10 0 1 0 .57-8.38" />
        </svg>
        <span>Reset</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Controls",
  props: ["paused"]
};
</script>

<style scoped>
#container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-left: 20px;
  padding: 14px 20px 14px 20px;
  border: 1px solid slateblue;
  border-radius: 10px;
}
button {
  width: 100px;
  height: 60px;
  margin: 5px;
  text-align: center;
  appearance: none;
  border: 1px solid slateblue;
  border-radius: 6px;
  background-color: #414144;
  color: #ddd;
}
span {
  display: block;
}
.paused {
  background-color: #54545c;
}
</style>
