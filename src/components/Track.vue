<template>
  <div>
    <div id="container">
      <Beat v-for="i in 32" :key="i" @box-clicked="$emit('box-click', i - 1)" />
      <button id="mute" @click="muteClick" :class="{ muted: muted }">Mute</button>
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
  data() {
    return {
      muted: false
    };
  },
  methods: {
    muteClick() {
      this.$emit("mute-click");
      this.muted = !this.muted;
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
#mute {
  margin: 2px;
  height: 22px;
}

.muted {
  background-color: red;
}
</style>