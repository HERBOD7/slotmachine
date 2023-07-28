<template>
  <div class="boxes">
    <div
      :class="['box', transitionend ? 'blur' : '']"
      v-for="(symbol, index) in symbols"
      :key="index"
    >
      {{ symbol }}
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    symbols: Array,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.boxes {
  transform: translateY(0);
  transition: transform 1s ease-in-out;
}

.box {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 3rem;
  height: 110px;
  width: 100px;
}
</style>
