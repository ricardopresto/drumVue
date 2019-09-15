<template>
  <div>
    <div id="box" v-on:click="boxClick" v-bind:class="{ select: selected}"></div>
  </div>
</template>

<script>
export default {
  name: "Beat",
  data() {
    return {
      selected: false
    };
  },
  props: {
    index: Number
  },
  methods: {
    boxClick() {
      this.$emit("box-clicked");
      this.selected = !this.selected;
    }
  }
};
</script>

<style scoped>
#box {
  width: 20px;
  height: 20px;
  margin: 2px;
  display: inline-block;
  border: 1px solid black;
}
.select {
  background-color: blue;
}
</style>