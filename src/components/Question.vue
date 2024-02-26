<script setup>
defineProps({
  question: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <h1 class="green">{{ question }}</h1>
</template>

<style scoped>
</style>
