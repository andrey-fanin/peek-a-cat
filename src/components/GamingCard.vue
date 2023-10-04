<script setup>
import { computed } from 'vue'

const props = defineProps({
  position: { type: Number, required: true },
  faceValue: { type: Number, required: true },
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
      {{ faceValue }} - {{ position }} - {{ matched }}
    </div>
    <div class="card-side card-side--back">back</div>
    {{ faceValue }}
  </div>
</template>

<style scoped lang="scss">
.card {
  width: 100px;
  height: 100px;
  border: 1px solid #000;
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
  &--front {
    background-color: #ff0000;
    position: absolute;
    rotate: y 180deg;
  }
  &--back {
    background-color: #0000ff;
  }
}
</style>
