<script setup>
import Step from '@/components/Step.vue';
import { ref } from 'vue';

const emit = defineEmits(['endSession']);
const props = defineProps({
  step: {
    type: Object,
    required: true,
  },
});

const question = ref(props.step.question);
const answers = ref(props.step.answers);

const handle = (next) => {
  question.value = next.question;
  answers.value = next.answers;
};

document.addEventListener('keydown', (event) => {
  if (event.code === 'Escape') {
    emit('endSession');
  }
});
</script>

<template>
  <div class="session">
    <div
      class="exit"
      @click="$emit('endSession')"
    >
      &lt;Esc&gt;
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 384 512"><path d="M342.6 150.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L192 210.7 86.6 105.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3L146.7 256 41.4 361.4c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L192 301.3 297.4 406.6c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L237.3 256 342.6 150.6z"/></svg>
    </div>
  <Step
    :answers="answers"
    :question="question"
    @go-to-page="handle"
  ></Step>
  </div>
</template>

<style scoped>
.exit {
  font-size: 1.6rem;
  display: flex;
  justify-content: center;
  position: absolute;
  top: 2.2rem;
  right: 4rem;
  cursor: pointer;
}

svg {
  padding-top: 0.2rem;
  margin-left: 1.5rem;
  stroke: var(--color-text);
  fill: var(--color-text);
}

</style>
