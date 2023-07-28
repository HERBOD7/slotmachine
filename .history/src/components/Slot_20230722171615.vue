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
<style scoped></style>
