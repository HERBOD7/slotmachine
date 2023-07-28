<template>
  <div id="app">
    <div class="doors">
      <div class="door" v-for="i in 3" :key="i">
        <transition v-on:enter="enterTransition" v-on:leave="leaveTransition">
          <div class="boxes">
            <div
              :class="['box', transitionend ? 'blur' : '']"
              v-for="(symbol, index) in symbols"
              :key="index"
            >
              {{ symbol }}
            </div>
          </div>
        </transition>
      </div>
    </div>

    <div class="buttons">
      <button @click="spin" id="spinner">Play</button>
      <button @click="init" id="reseter">Reset</button>
    </div>
  </div>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    // HelloWorld,
  },
  data() {
    return {
      symbols: ["🍋", "🍉", "🍒", "🍊"],
      transitionstart: false,
      transitionend: false,
    };
  },
  created() {
    this.init();
  },
  methods: {
    init(firstInit = true, groups = 1, duration = 1) {
      const doors = document.querySelectorAll(".door");
      for (const door of doors) {
        const boxes = door.querySelector(".boxes");
        const boxesClone = boxes.cloneNode(false);
        // const pool = ["❓"];
        const pool = [];

        if (!firstInit) {
          const arr = [];
          for (let n = 0; n < (groups > 0 ? groups : 1); n++) {
            arr.push(...this.symbols);
          }
          pool.push(...this.shuffle(arr));
          this.$set(this.symbols, 0, { id: 4, name: "Item 4" });

          boxesClone.addEventListener(
            "transitionstart",
            function () {
              door.dataset.spinned = "1";
              this.querySelectorAll(".box").forEach((box) => {
                box.style.filter = "blur(1px)";
              });
            },
            { once: true }
          );

          boxesClone.addEventListener(
            "transitionend",
            function () {
              this.querySelectorAll(".box").forEach((box) => {
                box.style.filter = "blur(0)";
                // if (index > 0) this.removeChild(box);
              });
            },
            { once: true }
          );
        }

        for (let i = pool.length - 1; i >= 0; i--) {
          const box = document.createElement("div");
          box.classList.add("box");
          box.style.width = door.clientWidth + "px";
          box.style.height = door.clientHeight + "px";
          box.textContent = pool[i];
          boxesClone.appendChild(box);
        }
        boxesClone.style.transitionDuration = `${duration > 0 ? duration : 1}s`;
        boxesClone.style.transform = `translateY(-${
          door.clientHeight * (pool.length - 1)
        }px)`;
        door.replaceChild(boxesClone, boxes);
      }
    },
    enterTransition() {
      this.transitionstart = true;
      console.log("start");
    },
    leaveTransition() {
      this.transitionend = true;
      console.log("end");
    },
    async spin() {
      this.init(false, 1, 2);
      const doors = document.querySelectorAll(".door");
      for (const door of doors) {
        const boxes = door.querySelector(".boxes");
        const duration = parseInt(boxes.style.transitionDuration);
        boxes.style.transform = "translateY(0)";
        await new Promise((resolve) => setTimeout(resolve, duration * 100));
      }
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
