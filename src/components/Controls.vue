<template>
  <div>
    <button @click="$emit('play-function')">
      <svg
        width="40"
        height="40"
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
        width="40"
        height="40"
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
        width="40"
        height="40"
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
        width="40"
        height="40"
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
  </div>
</template>

<script>
export default {
  name: "Controls",
  props: ["paused"]
};
</script>

<style scoped>
button {
  width: 100px;
  height: 60px;
  margin: 10px;
}
.paused {
  background-color: lightslategrey;
}
</style>
