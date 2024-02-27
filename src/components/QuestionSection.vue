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

const allQuestions = computed(() => {
  return questions([], props.allSteps);
});

const questions = (all, step) => {
  if (step.question === '') {
    return all;
  }

  all.push({ id: step.id, name: step.question });

  step.answers.map(child => {
    return questions(all, child.next);
  });

  return all;
};

const linking = ref(false);

const link = () => {
  linking.value = true;
};

const unlink = () => {
  linking.value = false;
};

const submitLink = (event) => {
  if (event.code === 'Enter') {
    link();
  }
};

const findById = (id, step) => {
  if (step.id === id) {
    return step;
  }

  const answers = step.answers || [];
  return answers.filter(child => {
    return findById(id, child.next);
  })[0];
};

const updateQuestion = (event) => {
  event.target.style.width = (event.target.value.length + 2) + 'ch';
  const step = {
    ...props.step,
    question: event.target.value,
  };
  emit('updateStep', step);
};

const addStep = (event) => {
  const step = findById(parseInt(event.target.value), props.allSteps);
  event.target.style.width = (event.target.value.length + 2) + 'ch';
  emit('updateStep', step.next);
  unlink();
};
</script>

<template>
  <div class="question-section">
    <div class="link-question">
      <svg
        class="link-icon selectable"
        tabindex="0"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 640 512"
        @click="link"
        @keyup="submitLink"
      >
        <path d="M579.8 267.7c56.5-56.5 56.5-148 0-204.5c-50-50-128.8-56.5-186.3-15.4l-1.6 1.1c-14.4 10.3-17.7 30.3-7.4 44.6s30.3 17.7 44.6 7.4l1.6-1.1c32.1-22.9 76-19.3 103.8 8.6c31.5 31.5 31.5 82.5 0 114L422.3 334.8c-31.5 31.5-82.5 31.5-114 0c-27.9-27.9-31.5-71.8-8.6-103.8l1.1-1.6c10.3-14.4 6.9-34.4-7.4-44.6s-34.4-6.9-44.6 7.4l-1.1 1.6C206.5 251.2 213 330 263 380c56.5 56.5 148 56.5 204.5 0L579.8 267.7zM60.2 244.3c-56.5 56.5-56.5 148 0 204.5c50 50 128.8 56.5 186.3 15.4l1.6-1.1c14.4-10.3 17.7-30.3 7.4-44.6s-30.3-17.7-44.6-7.4l-1.6 1.1c-32.1 22.9-76 19.3-103.8-8.6C74 372 74 321 105.5 289.5L217.7 177.2c31.5-31.5 82.5-31.5 114 0c27.9 27.9 31.5 71.8 8.6 103.9l-1.1 1.6c-10.3 14.4-6.9 34.4 7.4 44.6s34.4 6.9 44.6-7.4l1.1-1.6C433.5 260.8 427 182 377 132c-56.5-56.5-148-56.5-204.5 0L60.2 244.3z" />
      </svg>
    </div>
    <input
      class="selectable"
      type="text"
      :value="step.question"
      @change="updateQuestion"
      v-if="!linking"
    />
    <select
      v-else
      class="selectable input"
      @focusout="unlink"
      @change="addStep"
    >
      <option selected></option>
      <option
        v-for="question in allQuestions"
        :value="question.id"
      >{{ question.name }}
      </option>
    </select>
  </div>
</template>

<style scoped>
.question-section {
  position: relative;
}

.selectable {
  border: 1px solid transparent;
  border-radius: .3rem;
  padding-left: .4rem;
  padding-right: .4rem;
  display: inline;
}

.selectable:focus-visible, .selectable:active {
  outline: 1px solid var(--color-text);
}

input, select {
  background: var(--color-background);
  color: var(--color-text);
  font-size: inherit;
  border: none;
  resize: none;
  min-width: 5rem;
  max-width: 95%;
  font-family: inherit;
}

select.selectable {
  border: 1px solid var(--color-text);
  display: inline-block;
}

.link-question:after {
  content: 'Q:';
  position: absolute;
  left: 2rem;
}

svg {
  height: 1rem;
  cursor: pointer;
  padding: 0;
  fill: var(--green);
}

.link-question {
  display: inline-block;
  position: absolute;
  margin-left: -3.6rem;
}
</style>
