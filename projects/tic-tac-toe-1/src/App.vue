<script setup>
import {ref} from 'vue'
  const boxes = ref([
    { index: 1, X: 0, O: 0 },
    { index: 2, X: 0, O: 0 },
    { index: 3, X: 0, O: 0 },
    { index: 4, X: 0, O: 0 },
    { index: 5, X: 0, O: 0 },
    { index: 6, X: 0, O: 0 },
    { index: 7, X: 0, O: 0 },
    { index: 8, X: 0, O: 0 },
    { index: 9, X: 0, O: 0 },
  ])

  const possibleWinningPatterns = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
    [1, 4, 7],
    [2, 5, 8],
    [3, 6, 9],
    [1, 5, 9],
    [3, 5, 7]
  ]
  const playerTurn = ref('X')
  const winner = ref(null)

  const playATurn = (pos) => {
    const box = boxes.value.find(box => box.index === pos)
    if (!box.O && !box.X && !winner.value) {

    if (playerTurn.value === 'X') {
      box.X = 1
      if (hasPlayerWon()) {
        winner.value = 'X'
      }
      playerTurn.value = 'O'
    } else {
      box.O = 1
      if (hasPlayerWon()) {
        winner.value = 'O'
      }
      playerTurn.value = 'X'
    }
    }
  }

  const restartGame = () => {
    winner.value = null
    boxes.value = [
    { index: 1, X: 0, O: 0 },
    { index: 2, X: 0, O: 0 },
    { index: 3, X: 0, O: 0 },
    { index: 4, X: 0, O: 0 },
    { index: 5, X: 0, O: 0 },
    { index: 6, X: 0, O: 0 },
    { index: 7, X: 0, O: 0 },
    { index: 8, X: 0, O: 0 },
    { index: 9, X: 0, O: 0 },
  ]
  playerTurn.value = 'X'
  }

  const hasPlayerWon = () => {
    // Check if player has won
    for (const pattern of possibleWinningPatterns) {
      const playerMoves = boxes.value?.filter(box => box[playerTurn.value] === 1)?.map(box => box.index)
      const isWinner = pattern.every((value) => playerMoves.includes(value))
      if (isWinner) {
        return isWinner
      } 
    }
  }
</script>
<template>
  <div class="center grid place-items-center h-[100vh]">

<div class="grid-child text-center">
  <div class="text-white text-md mb-4">
    Player {{ playerTurn }}'s turn
  </div>
  <div class="boxes max-w-[150px] flex flex-wrap">
    <button 
      v-for="box in boxes" 
      :key="box.index" class="h-[50px] w-[50px] border-white border grid items-center text-white" 
      @click="playATurn(box.index)">
      <span v-if="box.O" class="text-bold text-xl text-green-400">
        O
      </span>
      <span v-else-if="box.X" class="text-bold text-xl text-orange-400">
        X
      </span>
    </button>
  </div>
  <div v-if="winner" class="winner-ctn">
  <h2 class="text-white mt-4">
    
      <span v-show="winner === 'O'" class="text-bold text-xl text-green-400">
        O
      </span>
      <span v-show="winner === 'X'" class="text-bold text-xl text-orange-400">
        X
      </span> 
      Wins</h2>

      <button class="bg-white px-2 rounded-lg mt-4" @click="restartGame">New Game</button>
  </div>
</div>
  </div>
</template>
