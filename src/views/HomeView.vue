<script setup>
import { ref } from 'vue';
import { RouterLink } from 'vue-router';

// State to control the current screen
const currentScreen = ref(1);

// Function to go to the next screen
function startGame() {
  currentScreen.value = 2;
}
</script>

<template>
  <main class="min-h-screen flex flex-col justify-center bg-gray-900 text-white">

    <!-- Screen 1: Welcome Screen -->
    <section v-if="currentScreen === 1" class="relative bg-cover bg-center h-screen flex items-center justify-center"
      style="background-image: url('https://source.unsplash.com/random/1600x900/?game');">
      <div class="absolute inset-0 bg-black opacity-50"></div> <!-- Overlay -->
      <div class="relative z-10 text-center px-6 sm:px-12 py-16 sm:py-32">
        <h1 class="text-4xl sm:text-5xl lg:text-6xl font-bold text-white mb-6 drop-shadow-lg">Mine Hunter Game</h1>
        <p class="text-lg sm:text-xl lg:text-2xl text-gray-300 mb-8">Test your luck and find the hidden treasures while
          avoiding the mines. Can you win the game?</p>
        <button @click="startGame"
          class="px-8 py-4 bg-blue-600 text-white font-semibold text-lg sm:text-xl rounded-lg hover:bg-blue-700 transition duration-300 shadow-lg transform hover:scale-105">
          Learn More
        </button>
      </div>
    </section>

    <!-- Screen 2: How to Play -->
    <section v-if="currentScreen === 2" class="py-16 bg-gray-800">
      <div class="max-w-screen-xl mx-auto px-6 sm:px-12">
        <h2 class="text-2xl sm:text-4xl font-bold text-center text-white mb-6">How to Play</h2>
        <div class="flex flex-wrap justify-center md:justify-between items-center space-y-6 md:space-y-0">
          <div class="w-full sm:w-1/2 lg:w-1/3 text-center">
            <h3 class="text-lg font-semibold text-blue-500 mb-4">Objective</h3>
            <p class="text-sm sm:text-lg text-gray-400">Find all the safe tiles while avoiding the mines. Each correct tile gives
              you a treasure, and wrong one ends the game!</p>
          </div>
          <div class="w-full sm:w-1/2 lg:w-1/3 text-center">
            <h3 class="text-lg font-semibold text-blue-500 mb-4">Bet to Play</h3>
            <p class="text-sm sm:text-lg text-gray-400">Set your bet and number of mines before starting the game. The higher your
              bet, the higher the reward if you win!</p>
          </div>
        </div>
        <div class="text-center mt-8">
          <RouterLink to="/game">
            <button @click="startGame"
              class=" px-4 py-2 sm:px-8 sm:py-4 bg-green-600 text-white font-semibold text-sm sm:text-xl rounded-lg hover:bg-green-700 transition duration-300 shadow-lg transform hover:scale-105">
              Start the Game
            </button>
          </RouterLink>
        </div>
      </div>
    </section>

  </main>
</template>

<style scoped>
</style>
