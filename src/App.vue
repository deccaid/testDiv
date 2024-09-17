<template>
  <div v-if="currentQuestion" class="kviz">
    <div class="kviz__info">
      <h1 class="kviz__title">Тестирование</h1>
      <div class="kviz__block">
        <h2 class="kviz__subtitle">{{ currentQuestion.question }}</h2>
        <ul>
          <li class="kviz__point" v-for="(answer, index) in currentQuestion.answers" :key="index">
            <label class="kviz__text">
              <input class="kviz__input" type="radio" :value="answer" v-model="selectedAnswer" @change="selectAnswer(selectedAnswer)">
              <span class="kviz__custom-radio"></span>
              {{ answer }}
            </label>
          </li>
        </ul>
      </div>
      <ProgressBar
      :currentQuestionIndex="currentQuestionIndex - 1"
      :totalQuestions="totalQuestions"
    />
    </div>
  </div>
  <div v-else class="answer">
    <div class="answer__blocks">
      <h2 class="answer__title">{{ resultTitle }}</h2>
      <p class="answer__text">{{ resultTextMain }}</p>
      <p class="answer__text">{{ resultText }}</p>
      <ul class="answer__list">
        <li 
          class="answer__block" 
          v-for="(question, index) in questions" 
          :key="index" 
          :class="{
            'correct-answer': selectedAnswers[index] === question.correctAnswer,
            'incorrect-answer': selectedAnswers[index] !== question.correctAnswer
          }"
        >
          <h4 class="answer__title-block">{{ question.question }}</h4>
          <p class="answer__info">{{ selectedAnswers[index] }}</p>
        </li>
      </ul>
      <button class="answer__button" v-if="hasIncorrectAnswers" @click="restartQuiz">Пройти еще раз</button>
    </div>
  </div>
</template>

<script>
import ProgressBar from './components/ProgressBar.vue';
import { questions } from './utils/constants';
export default {
  components: {
    ProgressBar,
  },
  data() {
    return {
      questions:questions,    
      currentQuestionIndex: 0,
      score: 0,
      selectedAnswers: [],
      selectedAnswer: null,
      totalQuestions: 11,

    };
  },
  computed: {
    currentQuestion() {
      return this.questions[this.currentQuestionIndex];
    },
    resultTitle() {
      if (this.score === this.questions.length) {
        return "Поздравляем!";
      } else if (this.score > 0) {
        return "Хороший результат!";
      } else {
        return "Упс :(";
      }
    },
    resultTextMain() {
  if (this.score === this.questions.length) {
    return "Вы правильно ответили на все вопросы.";
  } else if (this.score > 0) {
    return `Вы ответили правильно на ${this.score} вопрос${this.score > 1 ? 'а' : ''}.`;
  } else {
    return "Вы неправильно ответили на все вопросы.";
  }
},
    resultText() {
      if (this.score === this.questions.length) {
        return "Вы действительно отлично разбираетесь в IT. ";
      } else if (this.score > 0) {
        return "Так держать!";
      } else {
        return "Нужно подучить теорию.";
      }
    },
    hasIncorrectAnswers() {
      return this.selectedAnswers.some((answer, index) => answer !== this.questions[index].correctAnswer);
    },
    restartQuiz() {
      this.selectedAnswers = [];
      this.currentQuestionIndex = 0;
      this.score = 0;
      this.selectedAnswer = null;
    },
  },
  methods: {
    selectAnswer(answer) {
  this.selectedAnswers[this.currentQuestionIndex] = answer; 
  if (answer === this.currentQuestion.correctAnswer) {
    this.score++;
  }

  setTimeout(() => {
    this.currentQuestionIndex++; 
    if (this.currentQuestionIndex < this.questions.length) {
      this.selectedAnswer = null; 
    } else {
      this.selectedAnswer = null; 

    }
  }, 1000);
}
  },
};
</script>

<style>
</style>