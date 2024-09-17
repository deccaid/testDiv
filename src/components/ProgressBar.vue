<template>
  <div>
    <div class="progress__block">
      <p class="progress__text">0</p>
      <p class="progress__text">10</p>
    </div>
    <div class="progress-bar">
      <div class="progress-bar__fill" :style="{ width: progress + '%' }"></div>
      <div class="progress-bar__current-question" v-if="currentQuestionIndex !== -1" :style="{ left: progress + '%' }">
        {{ currentQuestionIndex + 1 }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestionIndex: {
      type: Number,
      required: true
    },
    totalQuestions: {
      type: Number,
      required: true
    }
  },
  computed: {
    progress() {
      return ((this.currentQuestionIndex + 1) / this.totalQuestions) * 100;
    }
  }
};
</script>