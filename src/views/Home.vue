<template>
  <div>
    <NavBar />
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
import NavBar from "@/components/NavBar.vue";

export default {
  name: "home",
  components: {
    ImposteurForm,
    NavBar
  }
};
</script>
