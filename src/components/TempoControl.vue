<template>
  <div>
    <div id="container">
      <div id="length">
        <span>Loop Length (16th notes)&ensp;</span>
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
      speedSlider: ""
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
  flex-direction: column;
  align-items: center;
  width: 420px;
  border: 1px solid slateblue;
  border-radius: 10px;
  padding: 10px;
  margin-top: 20px;
  font-size: 0.9em;
}
#tempo {
  margin: 5px;
}
input {
  width: 200px;
}
#length {
  margin: 5px;
}
#lengthpick {
  display: inline-block;
  border: 1px solid slateblue;
  border-radius: 10px;
  padding: 5px 0 5px 0;
  cursor: default;
}
.selected {
  background-color: slateblue;
}
#length16 {
  padding: 5px;
  border-radius: 6px 0 0 6px;
}
#length32 {
  padding: 5px;
  border-radius: 0 6px 6px 0;
}
</style>