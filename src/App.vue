<template>
  <main class="pt-8 text-center">
    <!-- Titolo -->
    <h1 class="mb-4 text-3xl font-bold uppercase">Tic Tac Toe</h1>
    <!-- Contenitore giallo -->
    <div id="square" class="border-2 rounded-lg py-4 px-4 mx-96">
      <!-- Giocatore/Turno -->
      <h3 class="text-xl text-white mb-4">Player {{ player }}'s turn</h3>
      <!-- Contenitore Griglia -->
      <div class="flex flex-col items-center mx-4 my-8">
        <!-- Quadranti Griglia -->
        <div v-for="(row, x) in board" :key="x" class="flex">
          <div v-for="(cell, y) in row" :key="y" @click="MakeMove(x, y)"
            :class="`border-2 border-white w-24 h-24 hover:bg-gray-700 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${cell === 'X' ? 'text-red-600' : 'text-blue-800'}`">
            {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
          </div>
        </div>
      </div>
      <!-- Dichiara vincitore -->
      <div class="text-center">
        <h2 v-if="winner" class="text-3xl font-bold mb-8">Player {{ winner }} wins!</h2>
        <!-- Tasto Restart -->
        <button @click="ResetGame"
          class="border-dashed border-2 px-4 py-2 rounded-lg uppercase font-bold hover:scale-105">Reset</button>
      </div>
    </div>
  </main>
</template>

<style>
body {
  background-color: black;
  color: #FACC15;
}

button {
  border-color: #FACC15;
}

#square {
  border-color: #FACC15;
}
</style>



<script setup>
import { ref, computed } from 'vue'
/* Chi sta giocando */
const player = ref('X');
/* [Griglia(celle)] */
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
])
/* Combinazioni vincenti */
const CalculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}

/* Manda a schermo il vincitore */
const winner = computed(() => CalculateWinner(board.value.flat()))

/* Fai la mossa + cambia giocatore */
const MakeMove = (x, y) => {
  if (winner.value) return

  if (board.value[x][y] !== '') return

  board.value[x][y] = player.value

  player.value = player.value === 'X' ? 'O' : 'X'
}
/* Funzione di restart */
const ResetGame = () => {
  board.value =
    [
      ['', '', ''],
      ['', '', ''],
      ['', '', ''],
    ]
  player.value = 'X'
}


</script>
