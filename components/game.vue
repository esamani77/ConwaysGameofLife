<script setup>
// defineProps({ canvas });
// import { ref } from "vue";
import { ref, onMounted } from "vue";

const gamefield = ref(null);
let canvas = null;
let ctx = null;
let cell_size = 5;
let dead_color = `#000`;
let alive_color = `#FFF`;
let cells_in_column = Math.floor(1400 / cell_size);
let cells_in_rows = Math.floor(500 / cell_size);
let active_array = new Array();
let inactive_array = [];
function color(status) {}
let arrayInitialization = () => {
  for (let i = 0; i < cells_in_rows; i++) {
    for (let j = 0; j < cells_in_column; j++) {
      active_array[i][j] = 0;
    }
  }
  inactive_array = active_array;
};

let arrayRandomize = () => {
  for (let i = 0; i < cells_in_rows; i++) {
    active_array[i] = [];
    for (let j = 0; j < cells_in_column; j++) {
      active_array[i][j] = 0;
      active_array[i][j] = Math.random() > 0.5 ? 1 : 0;
    }
  }

  inactive_array = active_array;
};

let fillArray = () => {
  for (let i = 0; i < cells_in_rows; i++) {
    for (let j = 0; j < cells_in_column; j++) {
      let color;
      if (active_array[i][j] == 1) color = alive_color;
      else color = dead_color;
      ctx.fillStyle = color;
      ctx.fillRect(j * cell_size, i * cell_size, cell_size, cell_size);
    }
  }
};

let setCellValueHelper = (row, col) => {
  try {
    return Boolean(active_array[row][col]) ? active_array[row][col] : 0;
  } catch {
    return 0;
  }
};

let countNeighbours = (row, col) => {
  let total_neighbours = 0;
  total_neighbours += setCellValueHelper(row - 1, col - 1);
  total_neighbours += setCellValueHelper(row - 1, col);
  total_neighbours += setCellValueHelper(row - 1, col + 1);
  total_neighbours += setCellValueHelper(row, col - 1);
  total_neighbours += setCellValueHelper(row, col + 1);
  total_neighbours += setCellValueHelper(row + 1, col - 1);
  total_neighbours += setCellValueHelper(row + 1, col);
  total_neighbours += setCellValueHelper(row + 1, col + 1);
  return total_neighbours;
};

let updateCellValue = (row, col) => {
  const total = countNeighbours(row, col);
  // cell with more than 4 or less then 3 neighbours dies. 1 => 0; 0 => 0
  if (total > 4 || total < 3) {
    return 0;
  }
  // dead cell with 3 neighbours becomes alive. 0 => 1
  else if (active_array[row][col] === 0 && total === 3) {
    return 1;
  }
  // or returning its status back. 0 => 0; 1 => 1
  else {
    return active_array[row][col];
  }
};

let updateLifeCycle = () => {
  for (let i = 0; i < cells_in_rows; i++) {
    for (let j = 0; j < cells_in_column; j++) {
      let new_state = updateCellValue(i, j);
      inactive_array[i][j] = new_state;
    }
  }
  active_array = inactive_array;
};

// let gameSetUp = () => {
//   arrayInitialization();
// };

let runGame = () => {
  updateLifeCycle();
  fillArray();
};
let asss = () => {
  canvas = gamefield.value;
  ctx = canvas.getContext("2d");
  arrayRandomize();
  fillArray();
  setInterval(() => {
    runGame();
  }, 1);
};
</script>
<template>
  <div>
    <button @click="asss">run game</button>
    <canvas ref="gamefield" id="gamefield" width="1400" height="500"></canvas>
  </div>
</template>
