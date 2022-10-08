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
const winPercentage = computed(()=>{
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
  localStorage.setItem('draws', draws.value)
  localStorage.setItem('losses', losses.value)
}

const LoadGame = () => {
  wins.value = localStorage.getItem('wins')
  draws.value = localStorage.getItem('draws')
  losses.value = localStorage.getItem('losses')
}

const ResetGame = () => {
  choice.value = null
  computerChoice.value = null
  verdict.value = null
}

onMounted(()=>{
  LoadGame()
  window.addEventListener ('keypress', e => {
    if (e.key === 'r') {
      ResetRound()
    }
  })
})
</script>

<template>
  <h1>Salut le monde !</h1>

</template>


