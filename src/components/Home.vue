<template>
  <div class="square">
    <div v-for="(item, index) in square" :key="index">
      <div @click="revertValue(index)">
        <div v-show="item !== 0">
          {{ item }}
        </div>
      </div>
    </div>
    <button @click="startNewGame">Новая игра</button>
    <span v-show="isEnd">Игра окончена</span>
    <span v-show="isWinner" class="winner">Победа: {{ win }}</span>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      isEnd: false,
      square: [0, 0, 0, 0, 0, 0, 0, 0, 0],
      isNext: true,
      isWinner: false,
      win: "",
    };
  },

  methods: {
    revertValue(id) {
      if (this.isWinner) return;
      // Если пустая клетка, добавляем в нее значение X или O
      if (this.square[id] === 0) {
        if (this.isNext) {
          this.$set(this.square, id, "X");
        } else {
          this.$set(this.square, id, "O");
        }
        this.isNext = !this.isNext;
      }
      this.IsOver();
      this.win = this.checkWinner();
    },

    IsOver() {
      let counter = 0;
      for (let i = 0; i < 9; ++i) {
        if (this.square[i] === 0) ++counter;
      }

      this.isEnd = counter <= 0;
    },

    startNewGame() {
      this.isEnd = false;
      this.isWinner = false;
      this.win = "";
      
      for (let i = 0; i < 9; ++i) {
        this.$set(this.square, i, 0);
      }
    },

    checkWinner() {
      for (let i = 0; i < 9; i += 3) {
        if (
          this.square[i] === this.square[i + 1] &&
          this.square[i] === this.square[i + 2] &&
          this.square[i] !== 0
        ) {
          this.isWinner = true;
          return this.square[i];
        }
      }

      for (let i = 0; i < 9; ++i) {
        if (
          this.square[i] === this.square[i + 3] &&
          this.square[i] === this.square[i + 6] &&
          this.square[i] !== 0
        ) {
          this.isWinner = true;
          return this.square[i];
        }
      }

      if (
        this.square[0] === this.square[4] &&
        this.square[0] === this.square[8] &&
        this.square[0] !== 0
      ) {
        this.isWinner = true;
        return this.square[4];
      }
      if (
        this.square[2] === this.square[4] &&
        this.square[2] === this.square[6] &&
        this.square[2] !== 0
      ) {
        this.isWinner = true;
        return this.square[4];
      }
    },
  },
};
</script>

<style scoped>
.square {
  margin: 50px auto;
  width: 200px;
  display: block;
}

.square > div > div {
  border: 1px solid #000;
  width: 50px;
  height: 50px;
  padding: 5px;
  text-align: center;
  float: left;
  font-size: 30pt;
  font-family: monospace;
}

button {
  margin: 10px 50px;
  padding: 10px;
  border-radius: 10px;
  border: 1px solid;
}

button:hover {
  cursor: pointer;
  background-color: #eee;
}

span {
  display: block;
  text-align: center;
}
</style>
