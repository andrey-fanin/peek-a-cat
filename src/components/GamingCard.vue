<script setup>
import { computed } from 'vue'

const props = defineProps({
  position: { type: Number, required: true },
  faceValue: { type: String, required: true },
  visible: { type: Boolean, default: false },
  matched: { type: Boolean, default: false }
})
const emit = defineEmits(['select-card'])

const flippedStyles = computed(() =>
  props.visible ? 'card--is-flipped' : ''
)

const selectCard = () => {
  emit('select-card', {
    position: props.position,
    faceValue: props.faceValue
  })
}
</script>

<template>
  <div @click="selectCard" :class="['card', flippedStyles]">
    <div class="card-side card-side--front">
      <img :src="`src/assets/${props.faceValue}.jpg`" alt="" />
    </div>
    <div class="card-side card-side--back">
      <img src="../assets/paws.jpg" alt="" />
    </div>
  </div>
</template>

<style scoped lang="scss">
.card {
  width: 100px;
  height: 100px;
  border: 1px solid transparent;
  border-radius: 10px;
  color: #fff;
  position: relative;
  transition: 0.5s rotate ease-in;
  transform-style: preserve-3d;

  &--is-flipped {
    rotate: y 180deg;
  }
}

.card-side {
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border: 1px solid transparent;
  border-radius: 10px;
  overflow: hidden;

  &--front {
    position: absolute;
    rotate: y 180deg;
  }
  &--back {
  }
  img {
    width: 100%;
  }
}
</style>
