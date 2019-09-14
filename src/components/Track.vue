<template>
  <div>
    <div id="container">
      <div id="box" v-for="i in 32" v-bind:key="i" v-on:click="boxClick"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Track",
  methods: {
    boxClick() {
      this.$emit("box-click", i);
    }
  }
};
</script>

<style scoped>
#box {
  width: 20px;
  height: 20px;
  background-color: aqua;
  margin: 2px;
  display: inline-block;
}
</style>