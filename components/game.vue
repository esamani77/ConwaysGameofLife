<script setup>
// defineProps({ canvas });
// import { ref } from "vue";
import { ref, onMounted } from "vue";

const gamefield = ref(null);
// const canvas = this.ref.gamefield;
// const ctx = canvas.getContext("2d");
let cell_size = 5;
let dead_color = `#181818`;
let alive_color = `#FF756B`;
let cells_in_column = Math.floor(1400 / cell_size);
let cells_in_rows = Math.floor(500 / cell_size);
let active_array = new Array();
let inactive_array = [];
function color(status) {}
let arrayInitialization = () => {
  for (let i = 0; i < this.cells_in_rows; i++) {
    this.active_array[i] = [];
    for (let j = 0; j < this.cells_in_column; j++) {
      this.active_array[i][j] = 0;
    }
  }
  this.inactive_array = this.active_array;
};

let arrayRandomize = () => {
  for (let i = 0; i < cells_in_rows; i++) {
    for (let j = 0; j < cells_in_column; j++) {
      console.log(active_array);
      active_array[i][j] = 0;
      active_array[i][j] = Math.random() > 0.5 ? 1 : 0;
    }
  }
};

let fillArray = () => {
  for (let i = 0; i < this.cells_in_rows; i++) {
    for (let j = 0; j < this.cells_in_column; j++) {
      let color;
      if (this.active_array[i][j] == 1) color = this.alive_color;
      else color = this.dead_color;
      ctx.fillStyle = color;
      ctx.fillRect(
        j * this.cell_size,
        i * this.cell_size,
        this.cell_size,
        this.cell_size
      );
    }
  }
};

let setCellValueHelper = (row, col) => {
  try {
    return this.active_array[row][col];
  } catch {
    return 0;
  }
};

let countNeighbours = (row, col) => {
  let total_neighbours = 0;
  total_neighbours += this.setCellValueHelper(row - 1, col - 1);
  total_neighbours += this.setCellValueHelper(row - 1, col);
  total_neighbours += this.setCellValueHelper(row - 1, col + 1);
  total_neighbours += this.setCellValueHelper(row, col - 1);
  total_neighbours += this.setCellValueHelper(row, col + 1);
  total_neighbours += this.setCellValueHelper(row + 1, col - 1);
  total_neighbours += this.setCellValueHelper(row + 1, col);
  total_neighbours += this.setCellValueHelper(row + 1, col + 1);
  return total_neighbours;
};

let updateCellValue = (row, col) => {
  const total = this.countNeighbours(row, col);
  // cell with more than 4 or less then 3 neighbours dies. 1 => 0; 0 => 0
  if (total > 4 || total < 3) {
    return 0;
  }
  // dead cell with 3 neighbours becomes alive. 0 => 1
  else if (this.active_array[row][col] === 0 && total === 3) {
    return 1;
  }
  // or returning its status back. 0 => 0; 1 => 1
  else {
    return this.active_array[row][col];
  }
};

let updateLifeCycle = () => {
  for (let i = 0; i < this.cells_in_rows; i++) {
    for (let j = 0; j < this.cells_in_column; j++) {
      let new_state = this.updateCellValue(i, j);
      this.inactive_array[i][j] = new_state;
    }
  }
  this.active_array = this.inactive_array;
};

let gameSetUp = () => {
  this.arrayInitialization();
};

let runGame = () => {
  this.updateLifeCycle()``;
  this.fillArray();
};
onMounted(() => {
  // the DOM element will be assigned to the ref after initial render
  console.log("gamefield.value"); // <div>This is a gamefield element</div>
  console.log(gamefield.value); // <div>This is a gamefield element</div>
});
let asss = () => {
  arrayRandomize();
  // this.fillArray();
  // setInterval(() => {
  //   this.runGame();
  // }, 1);
};
</script>
<template>
  <div>
    <button @click="asss">ssss</button>
    <canvas ref="gamefield" id="gamefield" width="1400" height="500"></canvas>
  </div>
</template>
