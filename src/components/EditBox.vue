<template>
  <div>
    <div id="container">
      <div id="volumeOuter" ref="volumeOuter" @click="volumeClick">
        <div id="volumeInner" ref="volumeInner"></div>
      </div>
      <div>
        <div class="timeShift" id="push" @click="pushClick">-</div>
        <div class="timeShift" id="adjustedTime">{{timeShifted}}</div>
        <div class="timeShift" id="pull" @click="pullClick">+</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "EditBox",
  data() {
    return {
      timeShifted: 0
    };
  },
  props: ["volume"],
  mounted() {
    this.$nextTick(() => {
      this.$refs.volumeInner.style.height = `${this.volume}px`;
    });
  },
  methods: {
    volumeClick(e) {
      const top = this.$refs.volumeOuter.getBoundingClientRect().top;
      let volHeight = 100 - (e.pageY - top);
      this.$refs.volumeInner.style.height = `${volHeight}px`;
      this.$emit("volume-change", volHeight);
    },
    pushClick() {
      this.timeShifted--;
      this.$emit("time-change", -1);
    },
    pullClick() {
      this.timeShifted++;
      this.$emit("time-change", +1);
    }
  }
};
</script>

<style scoped>
#container {
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 1px solid red;
}
#volumeOuter {
  width: 20px;
  height: 100px;
  margin: 1px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  border: 1px solid slateblue;
}
#volumeInner {
  width: 20px;
  background-color: slateblue;
}
.timeShift {
  height: 20px;
  width: 20px;
  border-radius: 4px;
  margin: 1px 0;
  text-align: center;
  cursor: default;
  background-color: royalblue;
}
</style>