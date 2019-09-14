<template>
  <div>
    <button v-on:click="$emit('play-function')">Play</button>
    <button v-on:click="$emit('stop-function')">Stop</button>
  </div>
</template>

<script>
export default {
  name: "Transport",
  props: {}
};
</script>

<style scoped>
button {
  width: 100px;
  height: 60px;
}
</style>
