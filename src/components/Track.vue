<template>
  <div>
    <div id="container">
      <Beat v-for="i in 32" :key="i" @box-clicked="$emit('box-click', i * 10 - 10)" />
    </div>
  </div>
</template>

<script>
import Beat from "./Beat.vue";

export default {
  name: "Track",
  components: {
    Beat
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
}
</style>