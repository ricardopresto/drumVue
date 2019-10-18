<template>
  <div>
    <div id="container">
      <div id="length">
        <span>Length&ensp;</span>
        <div id="lengthpick">
          <span
            id="length16"
            :class="{selected: length == 16}"
            @click="$emit('length-change', 16)"
          >16&nbsp;</span>
          <span
            id="length32"
            :class="{selected: length == 32}"
            @click="$emit('length-change', 32)"
          >&nbsp;32</span>
        </div>
      </div>
      <div id="tempo">
        <span>Tempo&ensp;</span>
        <input v-model="speedSlider" type="range" min="6" max="30" value="22" @change="speedChange" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TempoControl",
  data: function() {
    return {
      speedSlider: "22"
    };
  },
  props: ["length"],
  methods: {
    speedChange() {
      this.$emit("speed-change", this.speedSlider);
    }
  }
};
</script>

<style scoped>
#container {
  display: flex;
  flex-direction: row;
  align-items: center;
  width: 419px;
  border: 1px solid slateblue;
  border-radius: 10px;
  padding: 10px;
  margin-top: 20px;
  font-size: 0.9em;
  color: #ddd;
}
#tempo {
  display: flex;
  flex-direction: row;
  align-items: center;
  margin: 5px 20px;
}
input {
  width: 200px;
  -webkit-appearance: none;
  appearance: none;
  height: 16px;
  outline: none;
  background-color: #383838;
  border: 1px solid slateblue;
  border-radius: 6px;
}
input::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 14px;
  height: 14px;
  border-radius: 5px;
  background: slateblue;
  cursor: default;
}

input::-moz-range-thumb {
  width: 14px;
  height: 14px;
  border-radius: 5px;
  background: slateblue;
  cursor: default;
}
#length {
  display: flex;
  flex-direction: row;
  align-items: center;
  margin: 5px;
}
#lengthpick {
  display: inline-block;
  padding: 6px 0 6px 0;
  cursor: default;
}
.selected {
  background-color: slateblue;
}
#length16 {
  padding: 5px;
  border: 1px solid slateblue;

  border-radius: 8px 0 0 8px;
}
#length32 {
  padding: 5px;
  border: 1px solid slateblue;

  border-radius: 0 8px 8px 0;
}
</style>