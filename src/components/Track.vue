<template>
  <div>
    <div id="container">
      <Beat v-for="i in 32" :key="i" :index="i" @box-clicked="$emit('box-click', i)" />
    </div>
  </div>
</template>

<script>
import Beat from "./Beat.vue";

export default {
  name: "Track",
  components: {
    Beat
  },
  methods: {}
};
</script>

<style scoped>
#container {
  width: 100%;
  height: 20px;
  margin: 2px;
  display: flex;
  flex-direction: row;
}
</style>