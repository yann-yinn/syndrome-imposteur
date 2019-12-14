<template>
  <div class="imposteur-form">
    <template v-if="showResults === false">
      <div
        class="question"
        v-for="(question, questionIndex) in questions"
        :key="questionIndex"
      >
        <h2 class="title is-5">{{ question }}</h2>
        <div class="columns">
          <div
            v-for="(choice, choiceIndex) in choices"
            :key="choiceIndex"
            class="column has-text-centered question-choice"
          >
            <button
              class="button"
              :class="{
                'is-warning': choiceIsActive({
                  questionId: questionIndex,
                  answerId: choiceIndex + 1
                })
              }"
              @click="
                onChoiceClick({
                  questionId: questionIndex,
                  answerId: choiceIndex + 1
                })
              "
            >
              {{ choice }}
            </button>
          </div>
        </div>
      </div>
      <div style="padding-top:40px;" class="control has-text-centered">
        <button
          style="margin: 40px 0"
          @click="onSubmit"
          class="button is-large is-info"
        >
          CALCULER MON SCORE FINAL
        </button>
      </div>
    </template>

    <template v-if="showResults === true">
      <div class="button" @click="onBackClick">< retour au questionnaire</div>
      <div class="results">
        <p>score total: {{ result }}</p>
      </div>
    </template>
  </div>
</template>

<script>
import questions from "../questions";
const choices = [
  "Pas du tout vrai",
  "Rarement",
  "Parfois",
  "Souvent",
  "Très vrai"
];

export default {
  data() {
    return {
      answers: {},
      result: 0,
      showResults: false
    };
  },
  created() {
    this.questions = questions;
    this.choices = choices;
  },
  methods: {
    calculateScore() {
      this.showResults = true;
      let result = 0;
      for (let [, score] of Object.entries(this.answers)) {
        result = result + score;
      }
      this.result = result;
    },
    onChoiceClick({ questionId, answerId }) {
      this.answers = {
        ...this.answers,
        [questionId]: answerId
      };
    },
    choiceIsActive({ questionId, answerId }) {
      if (this.answers[questionId]) {
        if (answerId === this.answers[questionId]) {
          return true;
        }
      }
      return false;
    },
    onSubmit() {
      if (Object.keys(this.answers).length < questions.length) {
        const missingNumber =
          questions.length - Object.keys(this.answers).length;
        alert(
          `Il y encore ${missingNumber} question${
            missingNumber > 1 ? "s" : ""
          } sans réponse, vous devez répondre à toutes les questions pour que nous puissons calculer votre score. `
        );
      } else {
        this.calculateScore();
      }
    },
    onBackClick() {
      this.showResults = false;
    }
  }
};
</script>

<style scoped>
.imposteur-form {
  max-width: 700px;
  margin: auto;
}

.question {
  padding-top: 40px;
}

@media screen and (max-width: 769px) {
  .question-choice button {
    min-width: 300px;
  }
}

.results {
  font-size: 70px;
  padding: 40px;
}
</style>
