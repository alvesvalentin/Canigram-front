<script setup lang="ts">
import {computed, ref} from 'vue';

const poids = ref(70); // en kg
const age = ref(9) // en mois

const besoinsEnergetiques = computed(() => {
  if (!poids.value || !age.value) return 0;
  return (128 * Math.pow(poids.value, 0.73) * Math.pow(age.value, -0.05));
})

const besoinsProteines = computed(() => {
  if (!besoinsEnergetiques.value) return 0;
  return 3 * poids.value;
})

</script>

<template>
  <h1>Canigram</h1>

  <div class="dog-information">

    <div class="age-and-weight">
      <p>Poids idéal</p>
      <input type="number" v-model="poids"/>

      <p>Age (en mois)</p>
      <input type="number" v-model="age"/>

    </div>
  </div>

  <div class="results">
    <h3>Résultats</h3>

    <div class="needs">
      <span>Besoin énergétique</span> <span>{{ besoinsEnergetiques.toFixed(0) }}</span>

      <br>

      <span>Protéines</span> <span>{{besoinsProteines.toFixed(0)}}</span>

      <br>

      <span>Lipides</span> <span>Lipides</span>

      <br>

      <span>Glucides</span> <span>Glucides</span>
    </div>
  </div>


</template>

<style scoped>

</style>
