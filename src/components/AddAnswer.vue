<script setup>
import { computed, ref } from 'vue';

const emit = defineEmits(['updateStep']);
const props = defineProps({
  step: {
    type: Object,
    required: true,
  },
  allSteps: {
    type: Object,
    required: true,
  },
});

const error = ref(false);


const maxId = (current, step) => {
  if (step.answers.length === 0) {
    return Math.max(current, step.id);
  }

  return Math.max(...step.answers.map(item => maxId(current, item.next)));
};

const addAnswer = () => {
  const answers = props.step.answers || [];
  if (answers.length > 3) {
    error.value = true;
    setTimeout(() => error.value = false, 3000);
    return;
  }
  answers.push({
    answer: '',
    next: {
      question: '',
      id: maxId(0, props.allSteps) + 1,
      answers: []
    }
  });
  const step = {
    ...props.step,
    answers,
  };
  emit('updateStep', step);
};

const submitAdd = (event) => {
  if (event.code === 'Enter') {
    addAnswer();
  }
};
</script>

<template>
  <div
    class="add-answer"
  >
    <svg
      class="add-icon selectable"
      tabindex="0"
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 512 512"
      @click="addAnswer"
      @keyup="submitAdd"
    >
      <path d="M256 512A256 256 0 1 0 256 0a256 256 0 1 0 0 512zM232 344V280H168c-13.3 0-24-10.7-24-24s10.7-24 24-24h64V168c0-13.3 10.7-24 24-24s24 10.7 24 24v64h64c13.3 0 24 10.7 24 24s-10.7 24-24 24H280v64c0 13.3-10.7 24-24 24s-24-10.7-24-24z" />
    </svg>
    <p
      class="error"
      v-if="error"
    >
      More than four responses is too many — try a better question.
    </p>
  </div>
</template>

<style scoped>
.selectable {
  border: 1px solid transparent;
  border-radius: .3rem;
  padding-left: .4rem;
  padding-right: .4rem;
  display: inline;
}

.selectable:focus, .selectable:focus-visible {
  outline: 1px solid var(--color-text);
}

svg {
  height: 1rem;
  cursor: pointer;
  padding: 0;
}

svg.add-icon {
  fill: var(--color-text);
}

.error {
  color: var(--error);
  font-size: 0.9rem;
  display: flex;
}
</style>
