<template>
  <div>
    <div id="container">
      <Beat
        v-for="beat in trackArray"
        :key="beat.index"
        @box-clicked="$emit('box-click', beat.index)"
      />
      <button id="mute" @click="muteClick" :class="{ muted: muted }">Mute</button>
      <button id="edit" @click="editClick">Edit</button>
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
  props: ["trackArray"],
  methods: {
    muteClick() {
      this.$emit("mute-click");
      this.muted = !this.muted;
    },
    editClick() {
      this.$emit("edit-click");
    },
    noEditClick() {
      this.$emit("no-edit-click");
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
button {
  margin: 2px;
  height: 22px;
}

.muted {
  background-color: red;
}

.editing {
  background-color: slateblue;
}
</style>