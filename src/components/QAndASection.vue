<script setup>
import QuestionSection from '@/components/QuestionSection.vue';
import AnswerSection from '@/components/AnswerSection.vue';
import AddAnswer from '@/components/AddAnswer.vue';

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

const updateStep = (index, updated) => {
  const answers = props.step.answers;
  answers[index].next = updated;
  const step = {
    ...props.step,
    answers,
  };
  emit('updateStep', step);
};

</script>

<template>
  <div class="question-and-answer-section">
    <question-section
      :step="step"
      :all-steps="allSteps"
      @update-step="data => $emit('updateStep', data)"
    ></question-section>
    <div
      v-for="(answer, index) in step.answers"
    >
      <answer-section
        :step="step"
        :index="index"
        @update-step="data => $emit('updateStep', data)"
      ></answer-section>
      <div class="next" v-if="typeof answer !== 'undefined'">
        <q-and-a-section
          :step="answer.next"
          :all-steps="allSteps"
          @update-step="(updated) => updateStep(index, updated)"
        ></q-and-a-section>
      </div>
    </div>
    <add-answer
      :step="step"
      :all-steps="allSteps"
      @update-step="data => $emit('updateStep', data)"
    ></add-answer>
  </div>
</template>

<style scoped>
.question-and-answer-section {
  font-size: 1.2rem;
  width: 100%;
}

.next {
  margin-left: 3.4rem;
  position: relative;
}
</style>
