<script setup>
const emit = defineEmits(['updateStep']);
const props = defineProps({
  step: {
    type: Object,
    required: true,
  },
  index: {
    type: Number,
    required: true,
  },
});

const updateAnswer = (event) => {
  const answers = props.step.answers;
  answers[props.index].answer = event.target.value;
  event.target.style.width = (event.target.value.length + 2) + 'ch';
  const step = {
    ...props.step,
    answers,
  };
  emit('updateStep', step);
};

const removeAnswer = () => {
  const answers = props.step.answers;
  answers.splice(props.index, 1);
  const step = {
    ...props.step,
    answers,
  };
  emit('updateStep', step);
};

const vFocus = {
  mounted: (el) => el.focus()
}
</script>

<template>
  <div class="answer-section">
    <input
      class="selectable"
      type="text"
      :value="step.answers[index].answer"
      @change="updateAnswer"
      v-focus
    />
    <div
      class="remove"
      @click="removeAnswer"
    >
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512">
        <path d="M135.2 17.7L128 32H32C14.3 32 0 46.3 0 64S14.3 96 32 96H416c17.7 0 32-14.3 32-32s-14.3-32-32-32H320l-7.2-14.3C307.4 6.8 296.3 0 284.2 0H163.8c-12.1 0-23.2 6.8-28.6 17.7zM416 128H32L53.2 467c1.6 25.3 22.6 45 47.9 45H346.9c25.3 0 46.3-19.7 47.9-45L416 128z"/>
      </svg>
    </div>
  </div>
</template>

<style scoped>
.answer-section {
  margin-left: 2rem;
  position: relative;
}

input {
  background: var(--color-background);
  color: var(--color-text);
  font-size: inherit;
  border: none;
  resize: none;
  min-width: 5rem;
  max-width: 95%;
  font-family: inherit;
}

.remove:before {
  content: 'A:';
  position: absolute;
  left: -1.5rem;
}

.selectable {
  border: 1px solid transparent;
  border-radius: .3rem;
  padding-left: .4rem;
  padding-right: .4rem;
  display: inline;
}

.selectable:focus-visible {
  outline: 1px solid var(--color-text);
}

.remove {
  display: inline-block;
  margin-left: 0.5rem;
  cursor: pointer;
}

.remove svg {
  stroke: var(--error);
  fill: var(--error);
  height: 0.9rem;
}
</style>
