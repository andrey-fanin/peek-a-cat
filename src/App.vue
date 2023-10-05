<script setup>
import GamingCard from '@/components/GamingCard.vue'
import { computed, ref, watch } from 'vue'
import { shuffle } from '@/utils/shuffleArray'
import { launchConfetti } from '@/utils/confetti'

const newPlayer = ref(true)
const canFlipCard = ref(true)

const userSelection = ref([])
let cardList = ref([])
const cardItems = [0, 1, 2, 3, 4, 5, 6, 7]
// const cardItems = Array.from({ length: 8 })

//fill cardList.value with items from array
//position equals index of item in cardList.value
for (const item in cardItems) {
  const defaultCard = {
    faceValue: item,
    visible: false,
    matched: false
  }

  cardList.value.push(
    { ...defaultCard, position: item * 2, variant: 1 },
    {
      ...defaultCard,
      position: item * 2 + 1,
      variant: 2,
      visible: true
    }
  )
}

const status = computed(() => {
  if (remainingPairs.value === 0) {
    launchConfetti()
    return 'Player wins!'
  }
  return `Remaining Pairs: ${remainingPairs.value}`
})

const remainingPairs = computed(
  () => cardList.value.filter(({ matched }) => !matched).length / 2
)

const shuffleCards = () => {
  cardList.value = shuffle(cardList.value)
}

const startGame = () => {
  newPlayer.value = false
  restartGame()
}

const restartGame = () => {
  shuffleCards()
  cardList.value = cardList.value.map((card, index) => ({
    ...card,
    position: index,
    visible: false,
    matched: false
  }))
}

/**
 * Represents a flip of card.
 * If 2 cards are not matched, you can't select new card, until timeout is done.
 * @param card - item from cardList
 */

const flipCard = (card) => {
  if (canFlipCard.value) {
    cardList.value[card.position].visible = true

    if (userSelection.value[0]) {
      if (
        userSelection.value[0].position === card.position &&
        userSelection.value[0].faceValue === card.faceValue
      )
        return

      userSelection.value[1] = card
    } else {
      userSelection.value[0] = card
    }
  }
}

watch(
  userSelection,
  (currentValue) => {
    if (currentValue.length === 2) {
      const cardOne = currentValue[0]
      const cardTwo = currentValue[1]

      if (cardOne.faceValue === cardTwo.faceValue) {
        cardList.value[cardOne.position].matched = true
        cardList.value[cardTwo.position].matched = true
      } else {
        canFlipCard.value = false
        setTimeout(() => {
          cardList.value[cardOne.position].visible = false
          cardList.value[cardTwo.position].visible = false
          canFlipCard.value = true
        }, 1000)
      }

      userSelection.value = []
    }
  },
  { deep: true }
)
</script>

<template>
  <div class="wrapper">
    <h1>game!</h1>
    <transition-group tag="section" class="game-board" name="shuffle-card">
      <GamingCard
        v-for="card in cardList"
        :key="`${card.faceValue}-${card.variant}`"
        :face-value="card.faceValue"
        :visible="card.visible"
        @select-card="flipCard"
        :position="card.position"
      />
    </transition-group>
    <h2>
      {{ status }}
    </h2>
    <button v-if="newPlayer" @click="startGame">start!</button>
    <button v-else @click="restartGame">shuffle!</button>
  </div>
</template>

<style scoped lang="scss">
body {
  background-image: url('./assets/bg.jpg');
  background-position: left center;
  background-color: #565a5d;
}
.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  h1 {
    margin-inline: auto;
  }
  h2 {
    margin-inline: auto;
    font-size: 20px;
    font-family: 'Tahoma', sans-serif;
  }

  button {
    display: flex;
    align-items: center;
    justify-content: center;
    color: #9a6f5e;
    background-color: #42b8ac;
    padding: 5px 10px;
    outline: none;
    border-radius: 10px;
    border: 1px solid transparent;
    font-size: 20px;
    font-weight: bold;
    cursor: pointer;
    min-width: 100px;
  }
}

.game-board {
  width: calc(102px * 4 + 10px * 3);
  gap: 10px;
  margin-inline: auto;
  display: flex;
  flex: 1 1 25%;
  flex-wrap: wrap;
}

.shuffle-card-move {
  transition: 0.5s transform ease-in;
}
</style>
