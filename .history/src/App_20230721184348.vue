<template>
  <div id="app">
    <div class="slot-machine">
      <div class="row">
        <div
          class="column"
          v-for="(column, columnIndex) in columns"
          :key="columnIndex"
        >
          <div
            class="block"
            v-for="(symbol, symbolIndex) in column"
            :key="symbolIndex"
          >
            {{ symbol }}
          </div>
        </div>
      </div>
      <button @click="spin" :disabled="spinning">Spin</button>
      <p v-if="reward > 0">Congratulations! You won {{ reward }} credits!</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      symbols: ["cherry", "lemon", "orange", "watermelon"],
      columns: [[], [], []],
      spinning: false,
      reward: 0,
    };
  },
  methods: {
    spin() {
      if (this.spinning) return;

      this.spinning = true;
      this.reward = 0;

      const blocks = this.symbols.length;
      const spins = 3;

      for (
        let columnIndex = 0;
        columnIndex < this.columns.length;
        columnIndex++
      ) {
        const column = this.columns[columnIndex];
        column.length = 0; // Clear the column

        for (let i = 0; i < spins; i++) {
          const randomIndex = Math.floor(Math.random() * blocks);
          column.push(this.symbols[randomIndex]);
        }
      }

      setTimeout(() => {
        this.spinning = false;
        this.checkWin();
      }, 2000);
    },
    checkWin() {
      const uniqueSymbols = new Set();

      for (const column of this.columns) {
        uniqueSymbols.add(column[0]);
      }

      if (uniqueSymbols.size === 1) {
        const symbol = this.columns[0][0];
        switch (symbol) {
          case "cherry":
            this.reward = 10;
            break;
          case "lemon":
            this.reward = 20;
            break;
          case "orange":
            this.reward = 30;
            break;
          case "watermelon":
            this.reward = 40;
            break;
        }
      }
    },
  },
};
</script>

<style scoped>
.slot-machine {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: 24px;
}

.row {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.column {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0 10px;
}

.block {
  width: 100px;
  height: 100px;
  border: 1px solid black;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
