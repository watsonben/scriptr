<script setup>
import QAndASection from '@/components/QAndASection.vue';

const emit = defineEmits(['startSession', 'updateStep']);
defineProps({
  step: {
    type: Object,
    required: true,
  },
});

document.addEventListener('keydown', (event) => {
  if (event.code === 'Escape') {
    emit('startSession');
  }
});

const updateStep = (updated) => {
  emit('updateStep', updated);
};
</script>

<template>
  <div class="configure-page">
    <div
      class="session green"
      @click="$emit('startSession')"
    >
      &lt;Esc&gt;
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><path d="M438.6 278.6c12.5-12.5 12.5-32.8 0-45.3l-160-160c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3L338.8 224 32 224c-17.7 0-32 14.3-32 32s14.3 32 32 32l306.7 0L233.4 393.4c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0l160-160z"/></svg>
    </div>
    <div class="configure">
      <h2 class="green">Configure script</h2>
      <q-and-a-section
        :step="step"
        :all-steps="step"
        @update-step="updateStep"
      ></q-and-a-section>
    </div>
  </div>
</template>

<style scoped>
.configure-page {
  display: flex;
  justify-content: center;
}

.session {
  font-size: 1.6rem;
  display: flex;
  justify-content: center;
  position: absolute;
  top: 2rem;
  right: 4rem;
  cursor: pointer;
}

.configure {
  width: 50%;
  display: flex;
  align-items: start;
  flex-direction: column;
}

svg {
  padding-top: 0.2rem;
  margin-left: 1rem;
  stroke: var(--green);
  fill: var(--green);
}
</style>
