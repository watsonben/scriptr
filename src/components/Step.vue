<script setup>
import Answer from '@/components/Answer.vue';
import Question from '@/components/Question.vue';

const props = defineProps({
  question: {
    type: String,
    required: true
  },
  answers: {
    type: Array,
    default: [],
  }
});

const hasAnswer = (index) => typeof props.answers[index] !== 'undefined';
</script>

<template>
  <div class="step">
    <question :question="question"></question>
    <div class="answers" v-if="answers.length">
      <answer
        v-if="hasAnswer(1)"
        :answers="answers"
        :index="1"
        direction="Left"
        @go-to-page="(answer) => $emit('goToPage', answer)"
      ></answer>
      <answer
        v-if="hasAnswer(3)"
        :answers="answers"
        :index="3"
        direction="Up"
        @go-to-page="(answer) => $emit('goToPage', answer)"
      ></answer>
      <answer
        v-if="hasAnswer(2)"
        :answers="answers"
        :index="2"
        direction="Down"
        @go-to-page="(answer) => $emit('goToPage', answer)"
      ></answer>
      <answer
        v-if="hasAnswer(0)"
        :answers="answers"
        :index="0"
        direction="Right"
        @go-to-page="(answer) => $emit('goToPage', answer)"
      ></answer>
    </div>
  </div>
</template>

<style scoped>
.step {
  width: 100%;
  display: flex;
  flex-direction: column;
}
.answers {
  display: flex;
  width: 70%;
  justify-content: space-between;
  align-self: center;
}

</style>
