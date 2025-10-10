<template>
    <div
        class="min-h-screen flex flex-col items-center justify-center bg-gradient-to-b from-gray-900 to-gray-800 text-white px-4"
    >
        <!-- Título -->
        <h1 class="text-6xl font-bold mb-10">Tic-Tac-Toe</h1>

        <div class="grid md:grid-cols-2 gap-14">
            <!-- Área do jogo -->
            <div class="bg-gray-800/50 border w-80 border-gray-700 rounded-2xl p-6">
                <p class="text-lg font-semibold mb-4">Next player: {{ icon }}</p>
                <p class="text-lg font-semibold mb-4">
                    {{ squares }}
                    <!-- {{selectedCounter}} -->
                    <!-- {{playing}} -->
                </p>

                Counter: {{ counter }}
                <button class="text-lg font-semibold mb-4">+</button>

                <!-- Tabuleiro 3x3 -->
                <div class="grid grid-cols-3 gap-2">
                    <button
                        v-for="(square, squareIndex) in squares"
                        @click.prevent.stop="markSquare(squareIndex)"
                        :key="squareIndex"
                        class="aspect-square bg-gray-700 hover:bg-gray-600 rounded-xl text-3xl font-bold flex items-center justify-center transition"
                    >
                        {{ square }}
                    </button>
                </div>
            </div>

            <!-- Painel lateral -->
            <div class="bg-gray-800/50 border w-80 border-gray-700 rounded-2xl p-6 w-full max-w-sm">
                <h2 class="text-lg font-semibold mb-4">Game Settings</h2>

                <!-- Modo de jogo -->
                <div class="mb-6">
                    <label class="block text-sm font-medium mb-2">Select Game Mode:</label>
                    <div class="flex gap-2">
                        <button
                            class="flex-1 py-2 rounded-xl text-sm font-medium bg-blue-600 hover:bg-blue-500 transition"
                        >
                            Vs. Computer
                        </button>
                        <button
                            class="flex-1 py-2 rounded-xl text-sm font-medium bg-gray-700 hover:bg-gray-600 transition"
                        >
                            Vs. Human
                        </button>
                    </div>
                </div>

                <!-- Dificuldade -->
                <div class="mb-6">
                    <label class="block text-sm font-medium mb-2">AI Difficulty:</label>
                    <select
                        class="w-full rounded-xl bg-gray-700 border border-gray-600 py-2 px-3 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
                    >
                        <option>Easy</option>
                        <option>Medium</option>
                        <option selected>Hard</option>
                    </select>
                </div>

                <!-- Reset -->
                <button
                    class="w-full bg-green-600 hover:bg-green-500 text-white font-medium py-2 rounded-xl transition mb-8"
                >
                    Reset Game
                </button>

                <!-- Histórico -->
                <div>
                    <h3 class="text-md font-semibold mb-2">Game History</h3>
                    <button class="w-full bg-gray-700 hover:bg-gray-600 text-sm py-2 rounded-xl transition">
                        Go to game start
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>
<!-- TODO: Lógica de identificar o vencedor -->
<script setup lang="js">
const icons = ['X', 'O'];
const turn = ref(0);
import { showToast } from '~/utils/toast';

function anunciarVencedor(vencedor) {
    showToast(`🏆 ${vencedor} venceu a partida!`);
}
const winningPossibilities = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
];

const icon = computed(() => icons[turn.value]);

const squares = ref(['', '', '', '', '', '', '', '', '']);
const selectedCounter = computed(() => squares.value.filter((i) => i).length);
const playing = computed(() => Boolean(selectedCounter.value));

const markSquare = (position) => {
    if (typeof squares.value[position] === 'undefined' || squares.value[position]) {
        return;
    }
    squares.value[position] = icon.value;

    console.log([...squares.value]);
    turn.value = turn.value ? 0 : 1;
    let winner = checkWinner([...squares.value], winningPossibilities);
    if (winner) {
        anunciarVencedor(winner);
    }
};

const checkWinner = (ticTacToeBoard, winningCombos) => {
    for (let [a, b, c] of winningCombos) {
        if (ticTacToeBoard[a] && ticTacToeBoard[a] === ticTacToeBoard[b] && ticTacToeBoard[a] === ticTacToeBoard[c]) {
            console.log(ticTacToeBoard[a]);
            return ticTacToeBoard[a];
        }
    }
    return null;
};
</script>
