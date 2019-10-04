<template>
  <div id="container">
    <div id="box" v-for="i in 32" :key="i" :class="{ current: i == position + 1}" />
  </div>
</template>

<script>
export default {
  name: "Counter",
  props: ["position"]
};
</script>

<style scoped>
#container {
  width: 100%;
  height: 20px;
  margin: 10px;
  display: flex;
  flex-direction: row;
  margin-left: 140px;
}
#box {
  width: 20px;
  height: 6px;
  margin: 2px;
  display: inline-block;
  flex-shrink: 0;
  border: 1px solid slateblue;
}
.current {
  background-color: slateblue;
}
</style>