<template>
  <div>
    <header
      style="background:#74CFE2;padding:50px 0;color: #044389"
      class="has-text-centered"
    >
      <div class="titles">
        <h1 class="title is-1">Test du syndrome de l'imposteur</h1>
        <h2 class="subtitle">Échelle de Clance</h2>
      </div>
    </header>
    <div class="container">
      <ImposteurForm />
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
header {
  background: #74cfe2;
  padding: 50px 0;
}

header .title {
  color: #044389;
}

header .subtitle {
  color: ##044389;
}
</style>
