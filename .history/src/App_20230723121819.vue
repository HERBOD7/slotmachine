<template>
  <div id="app">
    <div class="slots">
      <div class="slot" v-for="(symbol, name, i) in symbols" :key="i">
        <SlotBox
          :symbols="symbol"
          :slotName="name"
          :slotIsShuffling="isShuffling"
        />
      </div>

      <div class="buttons">
        <div class="credit">{{ credit }}</div>
        <button @click="spin" class="spinner">Spin</button>
      </div>
    </div>
  </div>
</template>

<script>
import SlotBox from "./components/SlotBox.vue";

export default {
  name: "App",
  components: {
    SlotBox,
  },
  data() {
    return {
      symbols: {
        firstSlot: ["🍋", "🍉", "🍒", "🍊"],
        secondSlot: ["🍋", "🍉", "🍒", "🍊"],
        thirdSlot: ["🍋", "🍉", "🍒", "🍊"],
      },
      isShuffling: false,
      credit: 10,
    };
  },
  methods: {
    spin() {
      this.isShuffling = true;
      for (let name in this.symbols) {
        const arr = [];
        arr.push(...this.symbols[name]);
        this.symbols[name] = this.shuffle(arr);
      }
      setTimeout(() => {
        this.isShuffling = false;
      }, 100);
    },
    shuffle(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    },
  },
};
</script>

<style>
body {
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
}

#app {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: rgb(106, 0, 255);
  background: linear-gradient(
    261deg,
    rgba(106, 0, 255, 1) 0%,
    rgba(136, 0, 255, 1) 29%,
    rgba(252, 158, 69, 1) 100%
  );
}

.slots {
  display: flex;
  background: rgb(255, 222, 1);
  background: linear-gradient(
    261deg,
    rgba(255, 222, 1, 1) 0%,
    rgba(255, 185, 33, 1) 100%
  );
  padding: 10px;
  border-radius: 10px;
}

.slot {
  background: #fafafa;
  width: 100px;
  height: 110px;
  overflow: hidden;
  border-radius: 5px;
  margin: 2px;
}

.buttons {
  margin: 1rem 0 2rem 0;
  background: rgb(255, 222, 0);
  background: linear-gradient(
    85deg,
    rgba(255, 222, 0, 1) 0%,
    rgba(250, 189, 55, 1) 100%
  );
  display: flex;
  align-items: center;
  padding: 10px;
  border-radius: 10px;
}

button {
  cursor: pointer;
  font-size: 1.2rem;
  margin: 0 0.2rem;
  border: none;
  border-radius: 5px;
  padding: 10px 15px;
}

.credit {
  background: rgba(136, 0, 255, 1);
  padding: 5px;
  border-radius: 5px;
  color: white;
}

.spinner {
  height: 80px;
  width: 80px;
  border-radius: 100%;
  background: rgb(255, 0, 0);
  background-image: linear-gradient(
    to right,
    #e52d27 0%,
    #b31217 51%,
    #e52d27 100%
  );
  color: white;
  font-weight: bold;
}

.spinner:hover {
  background-position: right center;
}
</style>
