<script setup>
defineProps({ order: Number })
</script>

<template>
  <div class="card">{{ order }}</div>
</template>

<style scoped>
.card {
  width: 100px;
  height: 100px;
  border: 1px solid #000;
}
</style>
