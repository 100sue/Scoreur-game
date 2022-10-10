<script setup>
import {ref, computed, onMounted } from 'vue'

const wins = ref(0)
const drawns = ref(0)
const losses= ref(0)

const choice = ref(null)
const computerChoice = ref(null)
const verdict = ref(null)

const outcomes = {
  rock: {
    rock: 'drawn',
    paper: 'loss',
    scissors: 'win',
  },
  paper: {
    rock: 'win',
    paper: 'drawn',
    scissors: 'loss',
  },
  scissors: {
    rock: 'loss',
    paper: 'win',
    scissors: 'drawn',
  },

}
const winPercentage = computed(() => {
  const total = wins.value + drawns.value + losses.value
  return total ? (wins.value / total) * 100 : 0
})

const play = c => {
  choice.value = c

  const choices = ['rock', 'paper', 'scissors']
  const random = Math.floor(Math.random() * choices.length)
  computerChoice.value = choices[random]

  const outcome = outcomes[c][computerChoice.value]

  if (outcome === 'win') {
    wins.value++
    verdict.value = 'You win !'
  } else if (outcome === 'loss') {
    losses.value++
    verdict.value = 'You loses !'
  } else {
    drawns.value++
    verdict.value = 'It is a draw !'
  }
  SaveGame()

}

const SaveGame = () => {
  localStorage.setItem('wins', wins.value)
  localStorage.setItem('drawns', drawns.value)
  localStorage.setItem('losses', losses.value)
}

const LoadGame = () => {
  wins.value = parseInt(localStorage.getItem('wins')) || 0
  drawns.value = parseInt(localStorage.getItem('drawns')) || 0
  losses.value = parseInt(localStorage.getItem('losses')) || 0
}

const ResetRound = () => {
  choice.value = null
  computerChoice.value = null
  verdict.value = null
}

onMounted(() => {
  LoadGame()
  window.addEventListener ('keypress', e => {
    if (e.key === 'r') {
      ResetRound()
    }
  })
})
</script>

<template>
  <div class="bg-gray-700 text-white text-center min-h-screen flex flex-col">
    <header class="container mx-auto p-6">
      <h1 class="text-4x1 font-bold">Pierre, Feuille, Ciseaux</h1>
    </header>
    <main class="container mx-auto p-6 flex-1">
      <div v-if="choice === null" class="flex items-center justify-center -mx-6">

        <button @click="play('rock')" class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-blue-500">
        <img src="./assets/hand-closed-fist-outline-svgrepo-com.svg" alt="Rock" class="w-full">
        </button>

        <button @click="play('paper')" class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-green-500">
        <img src="./assets/hand-svgrepo-com.svg" alt="Paper" class="w-full">
        </button>

        <button @click="play('scissors')" class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-yellow-500">
        <img src="./assets/scissors-svgrepo-com.svg" alt="scissors" class="w-full">
        </button>
      </div>

      <div v-else>
        <div class="text-3x1 mb-4">
          You picked <span class="text-blue-500">{{ choice }}</span>
        </div>

        <div class="text-3x1 mb-4">
          The computer picked <span class="text-green-500">{{ computerChoice }}</span>
        </div>

        <div class="text-6x1 mb-12">{{ verdict }}</div>
        <button @click="ResetRound" class="bg-blue-500 text-lg py-2 px-4">Reset</button>
      </div>

      <div class="mt-12 text3x1 mb-4">{{ wins }} : {{ drawns }} : {{ losses }}</div>
      <div class="text"></div>


    </main>
  </div>

</template>


