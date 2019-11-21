<template>
  <div class="imposteur-form">
    <pre>
      score total: {{result}}
      </pre>
    <div class="question" v-for="(question, index) in questions" :key="index">
      <div class="question-title">{{question}}</div>
      <div class="question-choices">
        <div
          @click="onChoiceClick({questionId: index, answerId: 1})"
          :class="{'is-active': choiceIsActive({questionId: index, answerId: 1})}"
          class="question-choice"
        >1 - Pas du tout vrai</div>
        <div
          @click="onChoiceClick({questionId: index, answerId: 2})"
          :class="{'is-active': choiceIsActive({questionId: index, answerId: 2})}"
          class="question-choice"
        >2 - Rarement</div>
        <div
          @click="onChoiceClick({questionId: index, answerId: 3})"
          :class="{'is-active': choiceIsActive({questionId: index, answerId: 3})}"
          class="question-choice"
        >3 - Parfois</div>
        <div
          @click="onChoiceClick({questionId: index, answerId: 4})"
          :class="{'is-active': choiceIsActive({questionId: index, answerId: 4})}"
          class="question-choice"
        >4 - Souvent</div>
        <div
          @click="onChoiceClick({questionId: index, answerId: 5})"
          :class="{'is-active': choiceIsActive({questionId: index, answerId: 5})}"
          class="question-choice"
        >5 - Très vrai</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      answers: {},
      result: 0
    };
  },
  created() {
    this.questions = questions;
  },
  methods: {
    onChoiceClick({ questionId, answerId }) {
      this.answers = {
        ...this.answers,
        [questionId]: answerId
      };
      let result = 0;
      for (let [questionId, score] of Object.entries(this.answers)) {
        result = result + score;
      }
      this.result = result;
    },
    choiceIsActive({ questionId, answerId }) {
      console.log(questionId, answerId);
      if (this.answers[questionId]) {
        if (answerId === this.answers[questionId]) {
          return true;
        }
      }
      return false;
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
</style>