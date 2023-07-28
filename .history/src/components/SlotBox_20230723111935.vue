<template>
  <div class="boxes">
    <div
      :class="[
        'box',
        slotIsShuffling ? 'before-animate blur' : name + '-animation',
      ]"
      v-for="(item, index) in symbol"
      :key="index"
    >
      {{ item }}
    </div>
  </div>
</template>

<script>
export default {
  name: "SlotBox",
  props: {
    symbols: Array,
    slotIsShuffling: Boolean,
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

.firstSlot-animation {
  transform: translateY(3px);
  transition: transform 0.5s ease-in-out;
}

.secondSlot-animation {
  transform: translateY(3px);
  transition: transform 0.8s ease-in-out;
}

.thirdSlot-animation {
  transform: translateY(3px);
  transition: transform 1s ease-in-out;
}

.before-animate {
  transform: translateY(-20px);
}

.blur {
  filter: blur(1px);
}
</style>
