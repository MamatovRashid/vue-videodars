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
