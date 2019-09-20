<template>
  <div>
    <button @click="$emit('play-function')">Play</button>
    <button @click="$emit('stop-function')">Stop</button>
  </div>
</template>

<script>
export default {
  name: "Transport"
};
</script>

<style scoped>
button {
  width: 100px;
  height: 60px;
  margin: 10px;
}
</style>
