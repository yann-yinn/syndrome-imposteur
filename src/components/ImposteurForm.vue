<template>
  <div class="imposteur-form">
    <template v-if="showResults === false">
      <div class="content">
        <p style="font-weight:bold">Durée du test : 5 minutes - 20 questions</p>
        <p>
          Le
          <em>Test de l’Imposteur</em> a été développé pour aider les personnes
          à déterminer si elles ont ou non des caractéristiques du syndrome de
          l'imposteur et, si c’est le cas, à quel point elles en souffrent.
        </p>
        <p>
          Répondez le plus sincèrement et
          <strong>spontanément</strong> possible à toutes les questions (la
          première réponse qui vous vient est la bonne). Le résultat vous sera
          communiqué immédiatement !
        </p>
        <p class="help" style="font-style:italic">
          Tiré de The Impostor Phenomenon: When Success Makes You Feel Like A
          Fake, p20-22, P.R. Clance, 185 Toronto, Bantham Books. Coyright 1985
          Pauline Rose Clance, PhD, ABPP. Traduction française par Ars Maëlle du
          document disponible en ligne sur le site de l’auteur.
        </p>
      </div>
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
                  answerId: choiceIndex + 1,
                }),
              }"
              @click="
                onChoiceClick({
                  questionId: questionIndex,
                  answerId: choiceIndex + 1,
                })
              "
            >
              {{ choice }}
            </button>
          </div>
        </div>
      </div>

      <form @submit.prevent="onSubmit">
        <div style="padding-top:40px;" class="control">
          <h2 class="title is-5">Quel est votre âge ?</h2>
          <input
            class="input is-large"
            min="7"
            max="120"
            type="number"
            name="age"
            v-model="age"
            required
          />
        </div>
        <div style="padding-top:40px;" class="control has-text-centered">
          <button
            type="submit"
            style="margin: 40px 0"
            class="button is-large is-info submit"
          >
            CALCULER MON SCORE
          </button>
        </div>
      </form>
    </template>

    <template v-if="showResults === true">
      <h2 style="margin:40px 0" class="title is-1 has-text-centered">
        votre score : {{ result }} / 100
      </h2>

      <p>
        Plus le score est élevé, plus le syndrome de l'imposteur interfère
        fréquemment et lourdement dans la vie d’une personne.
      </p>

      <div style="margin:40px 0" class="results content">
        <h3 v-show="result < 41">
          Vous présentez peu de caractéristiques du syndrome de l’imposteur.
        </h3>
        <h3 v-show="result > 40 && result < 61">
          Vous présentez une expérience modérée du syndrome de l'imposteur.
        </h3>
        <h3 v-show="result > 60 && result < 81">
          Vous faites fréquemment l'expérience du syndrome de l'imposteur
        </h3>
        <h3 v-show="result > 80">
          Vous faites souvent d’intenses expériences du syndrome de l’imposteur.
        </h3>
      </div>

      <div style="margin:40px 0;" class="has-text-centered">
        <!-- appel déclic offert si resultat > 70%-->
        <a
          v-if="result > 69 && age > 20"
          href="https://calendly.com/aurelie-da-silva/appel-declic-45?utm_source=test-syndrome-imposteur"
          class="submit button call-to-action is-warning is-large"
        >
          Plus de 70/100? Réservez votre APPEL DECLIC offert
        </a>
        <a
          v-if="result < 70 || age < 21"
          href="https://aureliedasilva.podia.com/9-techniques-anti-syndrome-de-l-imposteur"
          class="submit button call-to-action is-warning is-large"
        >
          Découvrez 9 techniques contre-intuitives pour se libérer du syndrome
          de l’imposteur
        </a>
      </div>

      <div class="content">
        <p>
          Si le total est de
          <strong>40 ou moins</strong>, le répondant a peu de caractéristiques
          du syndrome de l'imposteur.
        </p>
        <p>
          Si le résultat est
          <strong>entre 41 et 60</strong>, le répondant a une expérience modérée
          du syndrome de l'imposteur.
        </p>
        <p>
          Un score
          <strong>entre 61 et 80</strong> signifie que le répondant fait
          fréquemment l'expérience du syndrome de l’imposteur.
        </p>
        <p>
          Un score
          <strong>supérieur à 80</strong> signifie que le répondant a souvent
          d’intenses expériences du syndrome de l'imposteur.
        </p>
        <p>
          Comment est calculé le score ? Chaque réponse apporte entre 1 (« pas
          du tout vrai ») à 4 points (très vrai).
        </p>

        <div style="margin-top:40px" class="has-text-centered">
          <button
            class="submit button call-to-action is-large"
            @click="onBackClick"
          >
            Revenir au questionnaire
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
  "Très vrai",
];

export default {
  data() {
    return {
      answers: {},
      result: 0,
      age: null,
      showResults: false,
    };
  },
  watch: {
    // pour le debug, on peut simuler un score obtenu
    // avec un param "score" dans l'url
    "$route.query.score": {
      immediate: true,
      handler: function(value) {
        if (value) {
          this.result = value;
          this.showResults = true;
        } else {
          this.result = 0;
          this.showResults = false;
        }
      },
    },
    // pour le debug, on peut ajouter l'âge dans l'url
    "$route.query.age": {
      immediate: true,
      handler: function(value) {
        if (value) {
          this.age = value;
        }
      },
    },
  },
  created() {
    this.questions = questions;
    this.choices = choices;
  },
  methods: {
    calculateScore() {
      let result = 0;
      for (let [, score] of Object.entries(this.answers)) {
        result = result + score;
      }
      this.result = result;
      this.showResults = true;
    },
    onChoiceClick({ questionId, answerId }) {
      this.answers = {
        ...this.answers,
        [questionId]: answerId,
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
        return;
      }
      if (!this.age) {
        alert("Le champ âge est obligatoire");
      }

      this.calculateScore();
    },
    onBackClick() {
      this.showResults = false;
    },
  },
};
</script>

<style scoped>
.question {
  padding-top: 20px;
}

.submit {
  white-space: unset;
  height: auto;
}

@media screen and (max-width: 769px) {
  .question-choice button {
    min-width: 200px;
  }
}
</style>
