<script setup>
import { ref } from 'vue';

const gridSize = 5;
const grid = ref([]);
const betAmount = ref(10);
const totalMines = ref(3);
const gameStarted = ref(false);
const gameOver = ref(false);
const gameStatus = ref('');

// Minimum bet amount and mines
const minBetAmount = 10;
const minMines = 1;

function startGame() {
    if (
        betAmount.value >= minBetAmount &&
        totalMines.value >= minMines &&
        totalMines.value < gridSize * gridSize
    ) {
        grid.value = Array.from({ length: gridSize * gridSize }, () => ({ revealed: false, isMine: false }));
        gameStarted.value = true;
        gameOver.value = false;
        gameStatus.value = '';

        let minesToPlace = totalMines.value;
        while (minesToPlace > 0) {
            const randomIndex = Math.floor(Math.random() * grid.value.length);
            if (!grid.value[randomIndex].isMine) {
                grid.value[randomIndex].isMine = true;
                minesToPlace--;
            }
        }
    } else {
        alert(`Please enter a bet amount of at least ${minBetAmount} and at least ${minMines} mine(s).`);
    }
}

function toggleTile(index) {
    if (gameOver.value || grid.value[index].revealed) return;

    grid.value[index].revealed = true;

    if (grid.value[index].isMine) {
        gameOver.value = true;
        gameStatus.value = "Game Over! You clicked on a mine. Try again!";
        revealAllTiles();
    } else if (grid.value.filter(tile => !tile.isMine && !tile.revealed).length === 0) {
        gameOver.value = true;
        gameStatus.value = "Congratulations! You won the game!";
        revealAllTiles();
    }
}

function revealAllTiles() {
    grid.value.forEach(tile => {
        tile.revealed = true;
    });
}

function halfBet() {
    if (betAmount.value > minBetAmount) {
        betAmount.value = Math.max(minBetAmount, Math.floor(betAmount.value / 2));
    }
}

function doubleBet() {
    betAmount.value = betAmount.value ? betAmount.value * 2 : minBetAmount;
}

function restartGame() {
    gameStarted.value = false;
    gameOver.value = false;
    gameStatus.value = '';
    betAmount.value = 10;
    totalMines.value = 3;
}

function goHome() {
    // Logic to go home (navigate to the homepage)
    // Example: router.push('/') if using Vue Router
}
</script>

<template>
    <main class="w-full min-h-screen flex flex-col items-center justify-center bg-gray-900 text-white p-4">
        <!-- Main Container -->
        <div
            class="flex flex-col md:flex-row bg-gray-800 shadow-lg rounded-lg overflow-hidden w-full max-w-full min-h-96">
            <!-- Sidebar (Game Settings) -->
            <aside v-if="!gameStarted" class="w-full md:w-96 p-3 bg-gray-700">


                <h2 class="text-lg font-semibold mb-4">Game Settings</h2>

                <div class="mb-4">
                    <label for="betAmount" class="block text-sm font-medium text-gray-300">Bet Amount (Min {{
                        minBetAmount}})</label>
                    <div class="flex items-center space-x-2 mt-1 w-full">
                        <button @click="halfBet"
                            class="px-3 py-1 bg-blue-500 text-white rounded-md hover:bg-blue-600 w-10 sm:w-12">-</button>
                        <input id="betAmount" v-model="betAmount" type="number" min="10" placeholder="Enter bet amount"
                            class="flex-1 px-3 py-2 border border-gray-500 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 bg-gray-800 text-white" />
                        <button @click="doubleBet"
                            class="px-3 py-1 bg-blue-500 text-white rounded-md hover:bg-blue-600 w-10 sm:w-12">+</button>
                    </div>
                </div>

                <div class="mb-4">
                    <label for="totalMines" class="block text-sm font-medium text-gray-300">Total Mines (Min {{ minMines
                        }}, Max
                        24)</label>
                    <input id="totalMines" v-model="totalMines" type="number" min="1" max="24"
                        placeholder="Enter total mines"
                        class="mt-1 w-full px-3 py-2 border border-gray-500 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 bg-gray-800 text-white" />
                </div>

                <button @click="startGame"
                    class="w-full py-2 mt-4 bg-blue-500 rounded-md text-white font-semibold hover:bg-blue-600">
                    Start Game
                </button>

                <!-- Go to Home Button -->
                <RouterLink to="/">
                    <button @click="goHome"
                        class="w-full py-2 mt-4 bg-gray-900 text-green-500 rounded-md font-semibold hover:bg-gray-600 mb-4">
                        <span class="flex flex-row items-center justify-center gap-2"><i
                                class="pi pi-arrow-circle-left"></i> Go to Home</span>
                    </button>
                </RouterLink>
            </aside>

            <!-- Grid -->
            <section class="flex-1 p-2">
                <div v-if="gameStarted" class="grid grid-cols-5 gap-4 place-items-center p-2 min-h-[calc(100vh-200px)]">
                    <div v-for="(tile, index) in grid" :key="index" @click="toggleTile(index)"
                        class="w-10 sm:w-12 md:w-16 h-10 sm:h-12 md:h-16 border border-gray-500 rounded-md flex items-center justify-center cursor-pointer transition-transform duration-300 transform"
                        :class="{ 'scale-110 bg-blue-500 text-white': tile.revealed, 'bg-red-500': tile.revealed && tile.isMine }">
                        <span v-if="tile.revealed">{{ tile.isMine ? '💣' : '💎' }}</span>
                    </div>
                </div>
                <p v-else class="text-gray-400 text-center">Set Bet Amount and Total Mines to start the game.</p>
            </section>
        </div>

        <!-- Game Status Message with Restart Button -->
        <div v-if="gameOver" class="mt-4 text-center text-lg font-semibold flex flex-col"
            :class="gameStatus.includes('Congratulations') ? 'text-green-500' : 'text-red-500'">
            {{ gameStatus }}
            <button @click="restartGame"
                class="mt-4 px-4 py-2 bg-green-500 rounded-md text-white font-semibold hover:bg-green-600">
                Restart Game
            </button>
        </div>
    </main>
</template>

<style scoped>
.grid>div {
    transition: transform 0.3s ease, background-color 0.3s ease;
}

@media (max-width: 768px) {
    .grid {
        grid-template-columns: repeat(5, 1fr);
    }
}
</style>
