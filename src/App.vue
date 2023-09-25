<script setup>
import GamingCard from '@/components/GamingCard.vue'
import { ref, watch } from 'vue'

const cardList = ref(
  Array.from({ length: 16 }, (x, i) => ({
    position: i++,
    order: i++,
    visible: false
  }))
)

const userSelection = ref([])

const flipCard = (card) => {
  cardList.value[card.position].visible = true

  if (userSelection.value[0]) {
    userSelection.value[1] = card
  } else {
    userSelection.value[0] = card
  }
}

watch(
  userSelection,
  (currentValue) => {
    if (currentValue.length === 2)
      userSelection.value.length = []
  },
  { deep: true }
)
</script>

<template>
  <div class="wrapper">
    <h1>game!</h1>
    <section class="game-board">
      <GamingCard
        v-for="card in cardList"
        :key="card.order"
        :order="card.order"
        :visible="card.visible"
        @select-card="flipCard"
        :position="card.position"
      />
    </section>
    {{ userSelection }}
  </div>
</template>

<style scoped lang="scss">
.wrapper {
  display: flex;
  flex-direction: column;

  h1 {
    margin-inline: auto;
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
</style>
