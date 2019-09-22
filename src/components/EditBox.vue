<template>
  <div id="container">
    <input
      type="range"
      orient="vertical"
      class="slider"
      min="0"
      max="100"
      @change="valChange"
      v-model="sliderVal"
    />
  </div>
</template>

<script>
export default {
  name: "EditBox",
  data: function() {
    return {
      sliderVal: "80"
    };
  },
  methods: {
    valChange() {
      this.$emit("volume-change", this.sliderVal);
    }
  }
};
</script>

<style scoped>
.slider {
  writing-mode: bt-lr;
  -webkit-appearance: slider-vertical;
  width: 20px;
  height: 100px;
  cursor: default;
}
</style>