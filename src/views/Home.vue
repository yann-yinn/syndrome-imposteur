<template>
  <div>
    <header class="has-text-centered">
      <div class="titles">
        <h1 class="title is-1">Test du syndrome de l'imposteur</h1>
        <h2 class="subtitle is-3">Échelle de Clance</h2>
      </div>
    </header>
    <div style="max-width: 700px" class="container">
      <div class="section">
        <ImposteurForm />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import ImposteurForm from "@/components/ImposteurForm.vue";

export default {
  name: "home",
  components: {
    ImposteurForm
  }
};
</script>

<style scoped>
@font-face {
  font-family: abril fatface;
  src: url("https://fonts.gstatic.com/s/abrilfatface/v11/zOL64pLDlL1D99S8g8PtiKchq-lmiA.ttf");
}
@font-face {
  font-family: josefin slab;
  src: url("https://fonts.gstatic.com/s/josefinslab/v10/lW-mwjwOK3Ps5GSJlNNkMalvESu6KerlFAke7w.woff2");
}
header {
  background-color: #74cfe2;
  padding: 100px 0;
  background-image: url("https://user-images.strikinglycdn.com/res/hrscywv4p/image/upload/c_limit,fl_lossy,h_9000,w_1200,f_auto,q_auto/1984359/761034_256831.png");
  background-repeat: no-repeat;
  background-position: top left;
  background-size: 400px;
  background-position: left 30px;
}

@media screen and (max-width: 769px) {
  header {
    background-position: center 30px;
    background-size: 140px;
    padding-top: 170px;
  }
}

header .title {
  max-width: 800px;
  margin: auto;
  font-family: "abril fatface", georgia, serif;
  color: #044389;
  font-weight: 400;
}

header .subtitle {
  font-family: "josefin slab", georgia, serif;
  padding-top: 50px;
  color: white;
}
</style>
