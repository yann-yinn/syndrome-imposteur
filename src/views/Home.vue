<template>
  <div class="section">
    <div class="container">
      <ImposteurForm />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import ImposteurForm from '@/components/ImposteurForm.vue'

export default {
  name: 'home',
  components: {
    ImposteurForm
  }
}
</script>
