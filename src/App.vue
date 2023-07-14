<script setup>
import { ref, computed } from "vue";

const player = ref('x');
const board = ref([
    ['','',''],
    ['','',''],
    ['','',''],
])

const calculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}

const winner = computed(() => calculateWinner(board.value.flat()))

const MakeMove = (x, y) => {
  if (winner.value) return

  if (board.value[x][y] !== '') return
  board.value[x][y] = player.value

  player.value = player.value === 'x' ? 'O' : 'x'
}

const ResetGame = () => {
  board.value = [
    ['','',''],
    ['','',''],
    ['','',''],
  ]
  player.value = 'x'
}
</script>

<template>
 <main class="pt-8 text-center dark:bg-cyan-900 min-h-screen dark:text-white">
   <h1 class="mb-6 text-3xl font-bold uppercase">Tic Tac Toe</h1>

   <h3 class="text-x1 mb-4">Player {{player}}'s turn</h3>

   <div class="flex flex-col items-center mb-8">
     <div
          v-for="(row, x) in board"
          :key="x"
          class="flex">
       <div
           v-for="(cell, y) in row"
            :key="y"
            @click="MakeMove(x, y)"
            :class="`border border-white w-20 h-20 hover:bg-cyan-800 flex items-center justify-center material-icons-outlined text-4x1 cursor-pointer ${ cell === 'x' ? 'text-pink-500' : 'text-blue-400'}`">
         {{ cell === 'x' ? 'close' : cell === 'O' ? 'circle' : ''}}
       </div>
     </div>
   </div>


   <h2 v-if="winner" class="text-6xl font-bold mb-8">Player '{{winner}}' wins!</h2>

   <button @click="ResetGame" class="px-4 py-2 bg-red-500 rounded uppercase font-bold hover:bg-red-600 duration-300">Reset Game</button>
 </main>

</template>

<style>

</style>
