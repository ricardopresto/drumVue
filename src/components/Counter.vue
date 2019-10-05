<template>
  <div id="container">
    <div id="box" v-for="i in length" :key="i" :class="{ current: i == position + 1, }" />
  </div>
</template>

<script>
export default {
  name: "Counter",
  props: ["position", "length"]
};
</script>

<style scoped>
#container {
  width: 100%;
  height: 20px;
  margin: 0 10px 10px 10px;
  display: flex;
  flex-direction: row;
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