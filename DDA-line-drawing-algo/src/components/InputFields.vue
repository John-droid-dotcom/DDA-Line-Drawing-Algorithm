<template>
  <form @submit.prevent="calculatePixel">
    <div class="flex justify-around items-center">
      <div class="flex flex-col items-start justify-center">
        <span>Starting Position</span>
        <div class="flex gap-4">
          <div class="flex gap-2">
            <label for="x-one">X-Co-ordinate:</label>
            <input
              type="number"
              name="x-one"
              v-model="firstX"
              class="border-2 rounded border-orange-400 focus:border-orange-200"
            />
          </div>
          <div class="flex gap-2">
            <label for="y-one">Y-Co-ordinate</label>
            <input
              class="border-2 rounded border-orange-400 focus:border-orange-200"
              type="number"
              v-model="firstY"
              name="y-one"
            />
          </div>
        </div>
      </div>

      <div class="flex flex-col items-start justify-center">
        <span>Ending Position</span>
        <div class="flex gap-4">
          <div class="flex gap-2">
            <label for="x-two">X-Co-ordinate:</label>
            <input
              type="number"
              name="x-two"
              v-model="secondX"
              class="border-2 rounded border-orange-400 focus:border-orange-200"
            />
          </div>
          <div class="flex gap-2">
            <label for="y-two">Y-Co-ordinate</label>
            <input
              class="border-2 rounded border-orange-400 focus:border-orange-200"
              type="number"
              v-model="secondY"
              name="y-two"
            />
          </div>
        </div>
      </div>
      <button class="bg-orange-400 px-4 py-2 text-white">Convert</button>
    </div>
  </form>

  <div class="flex" v-if="!rasterHidden" v-for="i in secondY">
    <div v-for="j in secondX">
      <div
        class="w-4 h-4 border border-black"
        :class="rows[j][i] ? 'bg-black' : ''"
      ></div>
    </div>
  </div>
</template>
<script>
export default {
  components: {},
  data() {
    return {
      firstX: 0,
      firstY: 0,
      secondX: 0,
      secondY: 0,
      difX: 0,
      difY: 0,
      steps: 0,
      yInc: 0,
      xInc: 0,
      rows: [],
      rasterHidden: true,
      bgColor: "bg-black",
    };
  },
  props: ["stepsTaken", "xIncrement", "yIncrement"],
  methods: {
    calculatePixel() {
      this.rows = [];
      this.difX = this.secondX - this.firstX;
      this.difY = this.secondY - this.firstY;
      this.steps = Math.max(Math.abs(this.difX), Math.abs(this.difY));

      this.xInc = this.difX / this.steps;
      this.yInc = this.difY / this.steps;

      for (let i = 0; i <= this.secondX; i++) {
        let row = [];
        for (let j = 0; j <= this.secondY; j++) {
          row.push(false);
        }
        this.rows.push(row);
      }
      console.log(
        this.rows,
        "original array",
        this.steps,
        this.xInc,
        this.yInc
      );

      let x = this.firstX;
      let y = this.firstY;
      for (let i = 0; i <= this.steps; i++) {
        this.rows[x][y] = true;
        console.log(x, y, "the increments");
        x = x + this.xInc;
        y = y + this.yInc;
        console.log(x, y, "the increments no round");

        x = Math.round(x);
        y = Math.round(y);
      }

      console.log(this.rows, "its here");
      this.rasterHidden = false;
    },
  },
};
</script>
<style></style>
