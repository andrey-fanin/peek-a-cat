<script setup>
const props = defineProps({
  position: { type: Number, required: true },
  faceValue: { type: Number, required: true },
  visible: { type: Boolean, default: false },
  matched: { type: Boolean, default: false }
})
const emit = defineEmits(['select-card'])

const selectCard = () => {
  emit('select-card', {
    position: props.position,
    faceValue: props.faceValue
  })
}
</script>

<template>
  <div class="card" @click="selectCard">
    <div v-show="visible" class="card-side card-side--front">
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
}
.card-side {
  width: 100%;
  height: 100%;
  &--front {
    background-color: #ff0000;
    position: absolute;
  }
  &--back {
    background-color: #0000ff;
  }
}
</style>
