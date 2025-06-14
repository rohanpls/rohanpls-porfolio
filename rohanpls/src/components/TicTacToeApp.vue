<template>
  <div class="tictactoe-container">
    <div class="status-header">
      <h2>{{ statusMessage }}</h2>
    </div>
    <div class="board">
      <div
        v-for="(cell, index) in board"
        :key="index"
        class="cell"
        :class="getCellClass(cell)"
        @click="humanMove(index)"
      >
        {{ cell }}
      </div>
    </div>
    <button @click="resetGame" class="reset-button">
      {{ winner || isDraw ? 'Play Again' : 'Reset Game' }}
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const HUMAN_PLAYER = 'X'
const AI_PLAYER = 'O'

const board = ref(Array(9).fill(null))
const winner = ref(null)
const isHumanTurn = ref(true) // Human always starts

const winningCombinations = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8], // rows
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8], // columns
  [0, 4, 8],
  [2, 4, 6], // diagonals
]

// A computed property for the status message for a better UX
const statusMessage = computed(() => {
  if (winner.value) return `Winner: ${winner.value === HUMAN_PLAYER ? 'You!' : 'Computer'}`
  if (isDraw.value) return "It's a Draw!"
  return isHumanTurn.value ? 'Your Turn (X)' : "Computer's Turn (O)"
})

const isDraw = computed(() => {
  return !winner.value && board.value.every((cell) => cell !== null)
})

const checkWinner = () => {
  for (const combination of winningCombinations) {
    const [a, b, c] = combination
    if (board.value[a] && board.value[a] === board.value[b] && board.value[a] === board.value[c]) {
      winner.value = board.value[a]
      return true
    }
  }
  return false
}

// --- UPDATED: This function now only handles the human's move ---
const humanMove = (index) => {
  if (!isHumanTurn.value || board.value[index] || winner.value) {
    return // Do nothing if it's not the human's turn, cell is taken, or game is over
  }

  // Human makes a move
  board.value[index] = HUMAN_PLAYER
  isHumanTurn.value = false

  // Check for winner, then trigger computer's move if the game isn't over
  if (!checkWinner() && !isDraw.value) {
    setTimeout(computerMove, 500) // Add a small delay for realism
  }
}

// --- NEW: The computer's AI logic ---
const computerMove = () => {
  if (winner.value) return

  // 1. Check if AI can win
  for (let i = 0; i < 9; i++) {
    if (board.value[i] === null) {
      board.value[i] = AI_PLAYER
      if (checkWinner()) {
        isHumanTurn.value = true
        return
      }
      board.value[i] = null // Backtrack
    }
  }

  // 2. Check if AI needs to block the human from winning
  for (let i = 0; i < 9; i++) {
    if (board.value[i] === null) {
      board.value[i] = HUMAN_PLAYER
      if (checkWinner()) {
        board.value[i] = AI_PLAYER // Block the spot
        winner.value = null // Reset winner check
        isHumanTurn.value = true
        return
      }
      board.value[i] = null // Backtrack
    }
  }
  winner.value = null // Important to reset after blocking check

  // 3. AI takes the best strategic spot
  const strategicMoves = [4, 0, 2, 6, 8, 1, 3, 5, 7] // Center, corners, then sides
  for (const move of strategicMoves) {
    if (board.value[move] === null) {
      board.value[move] = AI_PLAYER
      checkWinner()
      isHumanTurn.value = true
      return
    }
  }
}

const resetGame = () => {
  board.value.fill(null)
  isHumanTurn.value = true
  winner.value = null
}

const getCellClass = (cell) => {
  if (!cell) return ''
  return cell === 'X' ? 'cell-x' : 'cell-o'
}
</script>

<style scoped>
/* The styles from the previous version remain the same */
.tictactoe-container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #1e1e1e;
  font-family: 'Montserrat', sans-serif;
  color: white;
}
.status-header {
  margin-bottom: 20px;
  font-size: 1.5rem;
  min-height: 40px;
  text-align: center;
}
.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  gap: 10px;
}
.cell {
  width: 100px;
  height: 100px;
  background-color: #2c2c2c;
  border: 2px solid #444;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 3rem;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.2s;
}
.cell:hover {
  background-color: #3a3a3a;
}
.cell-x {
  color: #34d399;
}
.cell-o {
  color: #f87171;
}

.reset-button {
  margin-top: 30px;
  padding: 10px 20px;
  font-size: 1rem;
  font-weight: bold;
  color: white;
  background-color: #3b82f6;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.2s;
}
.reset-button:hover {
  background-color: #2563eb;
}
</style>
