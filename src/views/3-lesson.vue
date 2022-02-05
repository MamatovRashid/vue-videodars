<template>
  <div class="flex flex-col pt-20">
    <div class="flex justify-center mb-3">
      <p class="text-xl font-mono px-3">X: {{ wincounX }}</p>
      <p class="text-xl font-mono px-3">O: {{ wincounO }}</p>
    </div>
    <div class="box relative grid grid-cols-3 grid-rows-3 gap-0.5 bg-black mx-auto">
      <button class="w-52 h-52 bg-white text-6xl" :disabled="btn != ' '" ref="button" v-for="(btn, index) in buttons" :key="index" @click="handleClick(index)">{{ btn }}</button>
      <div class="absolute flex justify-center items-center w-full h-full bg-gray-50 bg-opacity-50" v-show="show">
        <div class="m-auto relative z-10 text-4xl text-gray-700 font-bold uppercase">{{ winnerName }}</div>
      </div>
    </div>
    <div class="text-center mt-3">
      <button class="bg-blue-600 text-white rounded px-3 py-1" @click="restart">Restart</button>
    </div>
  </div>
</template>

<script>
const X = "X";
const O = "O";
const EMPTY = " ";
export default {
  name: "TicTacToe",
  data() {
    return {
      turn: X,
      wincounX: 0,
      wincounO: 0,
      show: false,
      buttons: [EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY],
      winningLines: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ],
      winnerName: "",
      clicksCount: 0,
      object: {
        property: 10,
      },
    };
  },
  methods: {
    handleClick(index) {
      // this.buttons[index] = this.turn; not works in Vue 2
      this.$set(this.buttons, index, this.turn);
      // this.$set(this.object, property, 15);
      this.turn = this.turn === X ? O : X;
      this.clicksCount++;
      if (this.clicksCount >= 5) {
        this.checkForWin();
      }
    },
    checkForWin() {
      const isXWon = this.winningLines.some((line) => line.every((index) => this.buttons[index] === X));
      const isOWon = this.winningLines.some((line) => line.every((index) => this.buttons[index] === O));
      if (isXWon) {
        this.winnerName = "X yutdi!";
        this.wincounX++;
        this.show = true;
        setTimeout(() => {
          this.restart();
        }, 1000);
      }
      if (isOWon) {
        this.winnerName = "O yutdi!";
        this.wincounO++;
        this.show = true;
        setTimeout(() => {
          this.restart();
        }, 1000);
      }
      if (this.clicksCount === 9) {
        this.winnerName = "Durrang!";
        this.show = true;
        setTimeout(() => {
          this.restart();
        }, 1000);
      }
    },
    restart() {
      this.clicksCount = 0;
      this.buttons = this.buttons.map(() => EMPTY);
      this.show = false;
    },
  },
};
</script>

<style lang="scss"></style>

<!--
<template>
  <div class="w-96 mx-auto mt-20">
    <div class="flex my-2">
      <div class="w-1/2 text-2xl font-medium">X: {{ winnercountX }}</div>
      <div class="w-1/2 text-2xl font-medium">O: {{ winnercountO }}</div>
    </div>
    <div class="h-96 flex flex-wrap w-full -m-0.5 bg-black relative">
      <div class="p-0.5 w-1/3 h-1/3" v-for="(count, index) in counts" :key="index">
        <button :disabled="count.disabled" class="w-full h-full flex items-center justify-center bg-gray-50" @click="clickCell(index)">
          <span class="text-9xl leading-none font-rubik" :class="count.text == 'O' ? 'text-blue-500' : 'text-red-500'" v-if="count.disabled">{{ count.text }}</span>
        </button>
      </div>
      <div v-show="winner" class="absolute top-0 left-0 w-full h-full bg-black bg-opacity-50 flex items-center justify-center">
        <p  class="text-6xl text-green-600 bg-gray-100 px-3 py-1 rounded-md">{{ winner }}</p>
      </div>
    </div>
    <div>
      <button class="bg-blue-500 text-white px-3 text-xl py-1 mt-5 rounded" @click="reset()">Reset</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "ThirdLesson",
  data: () => ({
    winner: "",
    turn: true,
    count: 0,
    winnercountX: 0,
    winnercountO: 0,
    counts: [
      {
        disabled: false,
        text: "",
      },
      {
        disabled: false,
        text: "",
      },
      {
        disabled: false,
        text: "",
      },
      {
        disabled: false,
        text: "",
      },
      {
        disabled: false,
        text: "",
      },
      {
        disabled: false,
        text: "",
      },
      {
        disabled: false,
        text: "",
      },
      {
        disabled: false,
        text: "",
      },
      {
        disabled: false,
        text: "",
      },
    ],
    win: [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6],
    ],
    X: [],
    O: [],
  }),
  methods: {
    clickCell(id) {
      this.counts[id].disabled = true;
      if (this.turn) {
        this.X.push(id);
        this.counts[id].text = "X";
      } else {
        this.O.push(id);
        this.counts[id].text = "O";
      }
      this.turn = !this.turn;
      this.count++;
      if (this.count > 4) {
        let a = this.win.map((data) => data.every((number) => this.X.includes(number)));
        let b = this.win.map((data) => data.every((number) => this.O.includes(number)));
        if (a.find((data) => data === true)) {
          this.winner = "win X";
          this.winnercountX++;
          this.counts = this.counts.map((data) => {
            return { ...data, disabled: true };
          });
        } else if (b.find((data) => data === true)) {
          this.winner = "win O";
          this.winnercountO++;
          this.counts = this.counts.map((data) => {
            return { ...data, disabled: true };
          });
        }
      }
    },
    reset() {
      this.winner = "";
      this.turn = true;
      this.count = 0;
      this.counts = this.counts.map((data) => {
        return { ...data, disabled: false, text: "" };
      });
      this.X = [];
      this.O = [];
    },
  },
};
</script>

<style lang="scss" scoped></style>
-->
