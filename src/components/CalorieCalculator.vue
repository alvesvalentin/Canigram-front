<script setup lang="ts">
import {computed, ref} from 'vue';

const poids = ref(); // en kg
const age = ref(); // en mois
const stadeDeVie = ref(1.0); // Facteur de stade de vie par défaut
const activite = ref(1.8); // Facteur d'activité par défaut

const facteursActivite = [
  { label: 'Sédentaire (<1h de promenade en laisse)', value: 0.8 },
  { label: 'Actif (>1h de promenade sans laisse)', value: 1.0 },
  { label: 'Très Actif (Agility, Canicross, CaniVTT, chasse, mordant, Obéissance, ring  <7h par semaine)', value: 1.2 },
  { label: 'Athlète (Agility, Canicross, CaniVTT, chasse, mordant, Obéissance, ring  >7h par semaine)', value: 1.4 },
];

const facteurStadeDeVie = [
  { label: 'Chiot (2-4 mois)', value: 2.0 },
  { label: 'Chiot (4-7 mois)', value: 1.8 },
  { label: 'Chiot (7-8 mois)', value: 1.7 },
  { label: 'Adulte neutre, activité modérée', value: 1.6 },
  { label: 'Adulte très actif ou de travail', value: 2.0 },
  { label: 'Stérilisé ou sédentaire', value: 1.4 },
  { label: 'Âgé avec baisse d’activité', value: 1.2 },
];

const calculerBEM = computed(() => {
  if (!poids.value || !age.value) {
    return 0;
  }

  return 70 * Math.pow(poids.value, 0.75);
});

const calculerBEJ = computed(() => {
  if (!poids.value || !age.value) {
    return 0;
  }
  //return calculerBEM.value * activite.value;
  return calculBesoinsEnergetiques(poids.value, stadeDeVie.value, activite.value, 1.0);
});

function calculBesoinsEnergetiques(
    poids: number,
    stade: number,
    activite: number,
    environnement: number,
    autres: number = 1.0
): number {
  // Calcul du BER (Besoins Énergétiques au Repos)
  const BER = 110 * Math.pow(poids, 0.75);

  // Facteur total d'ajustement
  const facteurTotal = stade * activite * environnement * autres;

  // Calcul du BEQ (Besoins Énergétiques Quotidiens)
  return BER * facteurTotal;
}
</script>

<template>
  <div class="p-4 max-w-md mx-auto bg-white shadow-lg rounded-lg">
    <h2 class="text-xl font-bold mb-4">Calcul des besoins énergétiques</h2>
    <div class="aligné">
      <label class="block mb-2">Poids (kg)</label>
      <input v-model.number="poids" type="number" class="w-full border p-2 rounded mb-4" />

      <label class="block mb-2">Âge (mois)</label>
      <input v-model.number="age" type="number" class="w-full border p-2 rounded mb-4" />

      <label class="block mb-2">Taux d'activité</label>
      <select v-model.number="activite" class="w-full border p-2 rounded mb-4">
        <option v-for="facteur in facteursActivite" :key="facteur.value" :value="facteur.value">
          {{ facteur.label }}
        </option>
      </select>

      <label class="block mb-2">Stade de vie</label>
      <select v-model.number="stadeDeVie" class="w-full border p-2 rounded mb-4">
        <option v-for="facteur in facteurStadeDeVie" :key="facteur.value" :value="facteur.value">
          {{ facteur.label }}
        </option>
      </select>
    </div>
    <div class="mt-4 p-4 bg-gray-100 rounded">
      <p><strong>BEM :</strong> {{ calculerBEM.toFixed(2) }} kcal/jour</p>
      <p><strong>BEJ :</strong> {{ calculerBEJ.toFixed(2) }} kcal/jour</p>
    </div>

    <div class="mt-4 p-4 bg-gray-100 rounded">
      <p><strong>Viande :</strong> {{  }} kcal/jour</p>
      <p><strong>BEJ :</strong> {{ calculerBEJ.toFixed(2) }} kcal/jour</p>
    </div>
  </div>
</template>

<style scoped>
input, select {
  border: 1px solid #ccc;
  padding: 8px;
  border-radius: 4px;
}

.aligné {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

/* Pour Webkit (Chrome, Safari, Edge) */
input[type="number"]::-webkit-outer-spin-button,
input[type="number"]::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Pour Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}
</style>
