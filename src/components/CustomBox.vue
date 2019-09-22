<template>
  <div>
    <div id="outer" ref="outer" @click="volumeClick">
      <div id="inner" ref="inner"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CustomBox",
  props: {
    initialVol: Number
  },
  mounted() {
    this.$nextTick(() => {
      this.$refs.inner.style.height = `${this.initialVol}px`;
    });
  },
  methods: {
    volumeClick(e) {
      const top = this.$refs.outer.getBoundingClientRect().top;
      let volHeight = 100 - (e.pageY - top);
      this.$refs.inner.style.height = `${volHeight}px`;
      this.$emit("volume-change", volHeight);
    }
  }
};
</script>

<style scoped>
#outer {
  width: 20px;
  height: 100px;
  margin: 2px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  border: 1px solid slateblue;
}

#inner {
  width: 20px;
  background-color: slateblue;
}
</style>