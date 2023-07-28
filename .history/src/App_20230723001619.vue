<template>
  <div id="app">
    <div class="doors">
      <div class="door" v-for="(symbol, name, i) in symbols" :key="i">
        <div class="boxes">
          <div
            :class="[
              'box',
              isShuffling ? 'before-animate blur' : name + '-animation',
            ]"
            v-for="(item, index) in symbol"
            :key="index"
          >
            {{ item }}
          </div>
        </div>
      </div>
    </div>

    <div class="buttons">
      <button @click="spin" id="spinner">Play</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      symbols: {
        firstSlot: ["🍋", "🍉", "🍒", "🍊"],
        secondSlot: ["🍋", "🍉", "🍒", "🍊"],
        thirdSlot: ["🍋", "🍉", "🍒", "🍊"],
      },
      isShuffling: false,
    };
  },
  methods: {
    spin() {
      this.isShuffling = true;
      for (let name in this.symbols) {
        const arr = [];
        arr.push(...this.symbols[name]);
        this.symbols[name] = this.shuffle(arr);
        // this.isShuffling = true;
      }
      setTimeout(() => {
        this.isShuffling = false;
      }, 300);
      // this.isShuffling = false;
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
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
}

#app {
  width: 100%;
  height: 100%;
  background: #1a2b45;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.doors {
  display: flex;
}

.door {
  background: #fafafa;
  width: 100px;
  height: 110px;
  overflow: hidden;
  border-radius: 5px;
  margin: 5px;
}

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
  transition: transform 1s ease-in-out;
}

.secondSlot-animation {
  transform: translateY(3px);
  transition: transform 2s ease-in-out;
}

.thirdSlot-animation {
  transform: translateY(3px);
  transition: transform 3s ease-in-out;
}

.before-animate {
  transform: translateY(-20px);
}

.blur {
  filter: blur(1px);
}

.buttons {
  margin: 1rem 0 2rem 0;
}

button {
  cursor: pointer;
  font-size: 1.2rem;
  margin: 0 0.2rem;
  border: none;
  border-radius: 5px;
  padding: 10px 15px;
}

.info {
  position: fixed;
  bottom: 0;
  width: 100%;
  text-align: center;
}
</style>
