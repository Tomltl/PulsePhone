<template>
  <div class="flex flex-col items-center justify-center min-h-screen bg-gray-100">
    <h1 class="text-3xl font-bold mb-4">Recommandation de Téléphone</h1>
    <form @submit.prevent="submitForm" class="bg-white p-6 rounded-lg shadow-md">
      <div class="mb-4">
        <label for="budget" class="block text-gray-700">Budget :</label>
        <input
          v-model="budget"
          type="number"
          id="budget"
          class="mt-1 block w-full border border-gray-300 rounded-md p-2"
          required
        />
      </div>
      <div class="mb-4">
        <label for="screen_size" class="block text-gray-700">Taille de l'écran :</label>
        <input
          v-model="screenSize"
          type="number"
          id="screen_size"
          class="mt-1 block w-full border border-gray-300 rounded-md p-2"
          required
        />
      </div>
      <div class="mb-4">
        <label for="battery" class="block text-gray-700">Capacité de la batterie (mAh) :</label>
        <input
          v-model="battery"
          type="number"
          id="battery"
          class="mt-1 block w-full border border-gray-300 rounded-md p-2"
          required
        />
      </div>
      <button type="submit" class="w-full bg-blue-500 text-white py-2 rounded-md hover:bg-blue-600">
        Recommander un téléphone
      </button>
    </form>

    <div v-if="recommendedPhone" class="mt-6 p-4 bg-green-100 border border-green-400 rounded">
      <h2 class="font-bold">Téléphone recommandé :</h2>
      <p>Nom : {{ recommendedPhone.name }}</p>
      <p>Capacité de la batterie : {{ recommendedPhone.battery }} mAh</p>
      <p>Taille de l'écran : {{ recommendedPhone.screen }}</p>
      <p>Prix : {{ recommendedPhone.price }} €</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      budget: null,
      screenSize: null,
      battery: null,
      recommendedPhone: null
    }
  },
  methods: {
    async submitForm() {
      try {
        const response = await fetch('http://127.0.0.1:5000/recommend_phone', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            budget: this.budget,
            screen_size: this.screenSize,
            battery: this.battery
          })
        })

        if (!response.ok) {
          throw new Error('Erreur lors de la récupération des recommandations.')
        }

        this.recommendedPhone = await response.json()
      } catch (error) {
        console.error('Erreur:', error)
      }
    }
  }
}
</script>

<style>
/* Ajoute des styles personnalisés ici si nécessaire */
</style>
