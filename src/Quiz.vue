<template>
  <div v-if="!finished">
    <h1 class="header">Cognito Forms Quiz</h1>
    <div class="questionAnswer">
      <h4 class="questionText">{{ getCurrentQuestion.text }}</h4>
      <div
        class="answerOptions"
        v-for="(answer, index) in getCurrentQuestion.answers"
        :key="index"
      >
        <input
          class="radio"
          type="radio"
          :id="answer"
          :value="answer"
          v-model="selected"
        />
        <label class="label" :for="answer">{{ answer }}</label>
      </div>
      <button class="button" :disabled="!selected" @click="next">Next</button>
    </div>
  </div>
  <div v-else>
    <h1 class="header">Cognito Forms Quiz</h1>
    <h2 class="subheader">Quiz Complete!</h2>
    <div class="questionAnswer">
      <h3 class="white">Results:</h3>
      <ul>
        <li
          class="white noPoint"
          v-for="(answer, index) in answers"
          :key="index"
        >
          {{ questions[index].text }}: {{ answer }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import "./styles.css";
export default {
  props: {
    questions: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      // setting up state with variables we will need to track quiz progress
      currentQuestion: 0,
      answers: [],
      selected: null,
      finished: false,
    };
  },
  computed: {
    // computed property to get the current question
    getCurrentQuestion() {
      return this.questions[this.currentQuestion];
    },
  },
  methods: {
    next() {
      this.answers.push(this.selected);
      this.selected = null;
      // Make sure we aren't on the last question in array
      if (this.currentQuestion < this.questions.length - 1) {
        this.currentQuestion++;
      } else {
        this.finished = true;
      }
    },
  },
};
</script>
