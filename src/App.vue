<script setup>
import Card from '@/components/Card.vue'

const cardList = Array.from({ length: 16 }, (x, i) => i++)
</script>

<template>
  <div class="wrapper">
    <h1>game!</h1>
    <section class="game-board">
      <Card v-for="card in cardList" :key="card" :order="card" />
    </section>
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
