<script setup>
import { ref } from 'vue';
import Session from '@/components/Session.vue';
import Configure from '@/components/Configure.vue';

// TODO @Ben focus on new one
const inSession = ref(false);
const questions = ref({ question: '', id: 0, answers: [] });
</script>

<template>
  <div class="app">
    <Session
      v-if="inSession"
      :step="questions"
      @end-session="inSession = false"
    />
    <Configure
      v-else
      :step="questions"
      @start-session="inSession = true"
      @update-step="(step) => questions = step"
    ></Configure>
  </div>
</template>

<style>
.app {
  width: 100%;
}

p {
  font-size: 16pt;
}

h1 {
  text-align: center;
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  margin-bottom: 5rem;
  margin-top: 10rem;
}

h2 {
  text-align: center;
  font-weight: 400;
  font-size: 1.8rem;
  position: relative;
  margin-bottom: 1rem;
}

svg {
  height: 2.4rem;
}
</style>
