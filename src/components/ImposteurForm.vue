<template>
  <div class="imposteur-form">
    <template v-if="showResults === false">
      <div class="question" v-for="(question, index) in questions" :key="index">
        <div class="question-title">{{ question }}</div>
        <div class="question-choices">
          <div
            @click="onChoiceClick({ questionId: index, answerId: 1 })"
            :class="{
              'is-active': choiceIsActive({ questionId: index, answerId: 1 })
            }"
            class="question-choice"
          >
            1 - Pas du tout vrai
          </div>
          <div
            @click="onChoiceClick({ questionId: index, answerId: 2 })"
            :class="{
              'is-active': choiceIsActive({ questionId: index, answerId: 2 })
            }"
            class="question-choice"
          >
            2 - Rarement
          </div>
          <div
            @click="onChoiceClick({ questionId: index, answerId: 3 })"
            :class="{
              'is-active': choiceIsActive({ questionId: index, answerId: 3 })
            }"
            class="question-choice"
          >
            3 - Parfois
          </div>
          <div
            @click="onChoiceClick({ questionId: index, answerId: 4 })"
            :class="{
              'is-active': choiceIsActive({ questionId: index, answerId: 4 })
            }"
            class="question-choice"
          >
            4 - Souvent
          </div>
          <div
            @click="onChoiceClick({ questionId: index, answerId: 5 })"
            :class="{
              'is-active': choiceIsActive({ questionId: index, answerId: 5 })
            }"
            class="question-choice"
          >
            5 - Très vrai
          </div>
        </div>
      </div>
      <div style="padding-top:40px;" class="control has-text-centered">
        <button @click="onSubmit" class="button is-large is-info">
          Calculer mon score final
        </button>
      </div>
    </template>
    <template v-if="showResults === true">
      <div class="back" @click="onBackClick">< retour au questionnaire</div>
      <div class="results">
        <p>score total: {{ result }}</p>
        <p>
          <img src="https://media.giphy.com/media/3jmkremp3N2ow/giphy.gif" />
        </p>
      </div>
    </template>
  </div>
</template>

<script>
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
      this.calculateScore();
    },
    onBackClick() {
      this.showResults = false;
    }
  }
};

const questions = [
  "J'ai souvent réussi à un test ou à accomplir une tâche alors que j'avais peur de ne pas y arriver",
  "Je peux donner l'impression d'être plus compétent-e que je ne le suis vraiment",
  "J'évite les évaluations quand c'est possible et je suis terrifié-e que les autres m'évaluent",
  "Quand des gens me félicitent pour quelque chose que j'ai accompli, j'ai peur de ne pas être capable d'être à la hauteur de leurs attentes dans le futur"
];
</script>

<style scoped>
.imposteur-form {
  max-width: 700px;
  margin: auto;
}

.question {
  padding-top: 40px;
}
.question-choices {
  display: flex;
}

.question-choice {
  border: solid silver 1px;
  margin: 10px;
  padding: 10px;
  cursor: pointer;
}

.is-active {
  background: lightblue;
}

.results {
  font-size: 70px;
  padding: 40px;
}

.back {
  cursor: pointer;
}
</style>
