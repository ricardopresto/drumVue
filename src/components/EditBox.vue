<template>
  <div>
    <div id="container">
      <div id="volumeOuter" ref="volumeOuter" @click="volumeClick">
        <div id="volumeInner" ref="volumeInner"></div>
      </div>
      <div>
        <div class="timeShift" id="push" @click="pushClick" :class="{highlighted: timeShifted < 0}">
          <svg
            width="20"
            height="20"
            viewBox="0 0 24 24"
            fill="none"
            stroke="#ddd"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path d="M15 18l-6-6 6-6" />
          </svg>
        </div>
        <div class="timeShiftValue" id="adjustedTime">{{Math.abs(timeShifted)}}</div>
        <div class="timeShift" id="pull" @click="pullClick" :class="{highlighted: timeShifted > 0}">
          <svg
            width="20"
            height="20"
            viewBox="0 0 24 24"
            fill="none"
            stroke="#ddd"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path d="M9 18l6-6-6-6" />
          </svg>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "EditBox",
  props: ["volume", "timeShifted"],
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
      this.timeShifted > -9 ? this.$emit("time-change", -1) : null;
    },
    pullClick() {
      this.timeShifted < 9 ? this.$emit("time-change", +1) : null;
    }
  }
};
</script>

<style scoped>
#container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 20px;
  margin: 3px;
}
#volumeOuter {
  width: 20px;
  height: 100px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  border: 1px solid slateblue;
}
#volumeInner {
  width: 20px;
  background-color: #54497f;
}
.timeShift {
  height: 20px;
  width: 20px;
  border-radius: 4px;
  margin: 2px 0;
  text-align: center;
  cursor: default;
  background-color: #383838;
}
.timeShiftValue {
  height: 20px;
  width: 20px;
  border-radius: 4px;
  margin: 2px 0;
  text-align: center;
  cursor: default;
  font-family: Arial, Helvetica, sans-serif;
  background-color: slateblue;
}
.highlighted {
  background-color: slateblue;
}
</style>