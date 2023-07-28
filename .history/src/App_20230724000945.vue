<template>
  <div id="app">
    <div class="machine">
      <div class="slots">
        <div class="slot" v-for="(symbol, name, i) in symbols" :key="i">
          <SlotBox
            :symbols="symbol"
            :slotName="name"
            :slotIsShuffling="isShuffling"
          />
        </div>
      </div>

      <div class="buttons">
        <div class="credit">{{ credit }}</div>
        <button
          @click="spin"
          class="spinner"
          :disabled="!this.credit || isShuffling"
        >
          Spin
        </button>
        <button
          :disabled="isShuffling || credit === 0"
          :class="[{ unclickable: unclickable }, 'cashOut']"
          :style="{
            transform:
              `translateX(${cashOutTransition}px)` ||
              `translateY(${cashOutTransition}px)`,
          }"
          @mouseover="handleCashOutHover"
        >
          CASH OUT
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import SlotBox from "./components/SlotBox.vue";
import { rollsSymbols } from "./constant/rollsSymbols";

export default {
  name: "App",
  components: {
    SlotBox,
  },
  data() {
    return {
      symbols: rollsSymbols,
      isShuffling: false,
      credit: 10,
      audio: {
        spin: new Audio(
          "https://freesound.org/data/previews/120/120373_824230-lq.mp3"
        ),
        spinEnd: new Audio(
          "https://freesound.org/data/previews/145/145441_2615119-lq.mp3"
        ),
        win: new Audio(
          "https://freesound.org/data/previews/387/387232_1474204-lq.mp3"
        ),
      },
      unclickable: false,
      cashOutTransition: 0,
    };
  },
  methods: {
    spin() {
      if (this.credit) {
        this.credit--;
        this.audio.spin.play();
        this.isShuffling = true;
        let rolledSymbols = [];
        for (let name in this.symbols) {
          const arr = [];
          arr.push(...this.symbols[name]);
          const shuffleArr = this.shuffle(arr);
          this.symbols[name] = shuffleArr;
          rolledSymbols.push(shuffleArr[0]);
        }

        if (this.credit >= 40 && this.credit < 60) {
          const shouldReroll = Math.random() < 0.3;
          if (shouldReroll) {
            this.spin();
          }
        } else if (this.credit >= 60) {
          const shouldReroll = Math.random() < 0.6;
          if (shouldReroll) {
            this.spin();
          }
        }

        if (
          rolledSymbols[0].value === rolledSymbols[1].value &&
          rolledSymbols[1].value === rolledSymbols[2].value
        ) {
          console.log(this.credit, rolledSymbols[0].value);
          this.audio.win.play();
          this.credit = this.credit + rolledSymbols[0].value;
        }

        setTimeout(() => {
          this.audio.spin.pause();
          this.isShuffling = false;
          this.audio.spinEnd.play();
        }, 500);
        setTimeout(() => {
          this.audio.spinEnd.pause();
        }, 450);
      }
    },
    shuffle(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    },
    handleCashOutHover() {
      const randomDirection = Math.random() < 0.5 ? -1 : 1;
      const shouldMove = Math.random() < 0.5;
      const shouldDisable = Math.random() < 0.4;

      if (shouldMove) {
        this.cashOutTransition = randomDirection * 300;
      }

      if (shouldDisable) {
        this.unclickable = true;
      }
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

.machine {
  background: rgb(255, 222, 1);
  background: linear-gradient(
    261deg,
    rgba(255, 222, 1, 1) 0%,
    rgba(255, 185, 33, 1) 100%
  );
  padding: 10px;
  border-radius: 10px;
}

.slots {
  display: flex;
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
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
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
  background: rgb(149, 0, 255);
  background: linear-gradient(
    85deg,
    rgba(149, 0, 255, 1) 0%,
    rgba(235, 55, 250, 1) 100%
  );
  padding: 5px;
  border-radius: 5px;
  color: white;
  width: 30%;
  font-size: 20px;
  font-weight: bold;
}

.spinner {
  height: 80px;
  width: 80px;
  border-radius: 100%;
  background-image: linear-gradient(
    to right,
    #e52d27 0%,
    #b31217 51%,
    #e52d27 100%
  );
  color: white;
  font-weight: bold;
  transition: 0.5s;
  background-size: 200% auto;
}

.spinner:hover {
  background-position: right center;
}

.cashOut {
  height: 80px;
  width: 80px;
  font-size: 14px;
  border-radius: 50%;
}

.unclickable {
  pointer-events: none;
}
</style>
