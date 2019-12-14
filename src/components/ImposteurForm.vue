<template>
  <div class="imposteur-form">
    <template v-if="showResults === false">
      <p>
        Le <em>Test de l’Imposteur</em> a été développé pour aider les personnes
        à déterminer si elles ont ou non des caractéristiques du Phénomène de
        l’Imposteur et, si c’est le cas, à quel point elles en souffrent.
      </p>
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
          class="button is-large is-info submit"
        >
          CALCULER MON SCORE
        </button>
      </div>
    </template>

    <template v-if="showResults === true">
      <h2 style="margin:40px 0" class="title is-1 has-text-centered">
        votre score : {{ result }} / 100
      </h2>

      <p>
        Plus le score est élevé, plus le phénomène de l’imposteur interfère
        fréquemment et lourdement dans la vie d’une personne.
      </p>

      <div style="margin:40px 0" class="results content">
        <h3 v-show="result < 41">
          Vous présentez peu de caractéristiques de l’Imposteur.
        </h3>
        <h3 v-show="result > 40 && result < 61">
          Vous présentez une expérience modérée du phénomène de l’imposteur.
        </h3>
        <h3 v-show="result > 60">
          Vous faites souvent d’intenses expériences du phénomène de
          l’Imposteur.
        </h3>
      </div>

      <div class="content">
        <p>
          Si le total est de 40 ou moins, le répondant a peu de caractéristiques
          de l’Imposteur ; si le résultat est entre 41 et 60, le répondant a une
          expérience modérée du phénomène de l’imposteur ; un score entre 61 et
          80 signifie que le répondant a des sentiment d’imposteur ; et un score
          supérieur à 80 signifie que le répondant a souvent d’intenses
          expériences du phénomène de l’Imposteur. Plus le score est élevé, plus
          le Phénomène de l’imposteur interfère fréquemment et lourdement dans
          la vie d’une personne.
        </p>
        <div style="margin-top:40px" class="has-text-centered">
          <button class="submit button is-info is-large" @click="onBackClick">
            Retour au questionnaire
          </button>
        </div>
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
.question {
  padding-top: 40px;
}

@media screen and (max-width: 769px) {
  .submit {
    font-size: 19px;
  }
  .question-choice button {
    min-width: 200px;
  }
}
</style>
