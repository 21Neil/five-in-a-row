<template>
  <div
    class="stone"
    v-if="data !== 0"
    :class="data === 1 ? 'white' : 'black'"
  ></div>
</template>

<script>
export default {
  name: "Stone",
  props: {
    data: Number,
  },
};
</script>

<style scoped>
.stone {
  width: 35px;
  height: 35px;
  border-radius: 50%;
  margin: auto;
}
.white {
  background: rgb(255, 255, 255);
}
.black {
  background: #000;
}
</style>
