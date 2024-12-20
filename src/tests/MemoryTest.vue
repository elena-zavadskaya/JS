<template>
  <div class="memory-game">
    <h1>Тест на память</h1>

    <div v-if="!gameStarted">
      <p>Правила:</p>
      <ul>
        <li>Запоминайте последовательность чисел, которые будут появляться на экране.</li>
        <li>После показа последовательности введите их в обратном порядке.</li>
        <li>Числа вводите через пробел.</li>
        <li>Всего 9 раундов, сложность увеличивается с каждым этапом.</li>
      </ul>
      <button @click="startGame">Старт</button>
    </div>

    <div v-if="gameStarted && !gameOver">
      <p>Набор {{ round }} из 9</p>
    </div>

    <div v-if="showSequence">
      <p>Запоминайте числа:</p>
      <div class="number">{{ currentNumber }}</div>
    </div>

    <div v-if="!showSequence && !gameOver && gameStarted">
      <p>Введите последовательность в обратном порядке через пробел:</p>
      <input v-model="userInput" placeholder="Введите числа через пробел" />
      <button @click="checkInput">Проверить</button>
    </div>

    <div v-if="gameOver">
      <p>Ваш результат: {{ score }} / {{ maxScore }}</p>
      <button @click="resetGame">Начать заново</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sequence: [],
      currentNumber: null,
      userInput: "",
      showSequence: false,
      gameStarted: false,
      gameOver: false,
      score: 0,
      round: 1,
      maxRounds: 9,
      maxScore: 0,
      currentIndex: 0,
    };
  },
  methods: {
    generateSequence() {
      let length;
      if (this.round <= 3) {
        length = 3;
      } else if (this.round <= 6) {
        length = 4;
      } else {
        length = 5;
      }
      this.sequence = Array.from({ length }, () => Math.floor(Math.random() * 10));
      this.maxScore += length;
    },
    displaySequence() {
      this.currentIndex = 0;
      this.showSequence = true;

      const interval = setInterval(() => {
        if (this.currentIndex < this.sequence.length) {
          this.currentNumber = this.sequence[this.currentIndex];
          this.currentIndex++;
        } else {
          clearInterval(interval);
          this.showSequence = false;
          this.currentNumber = null;
        }
      }, 1000);
    },
    startGame() {
      this.gameStarted = true;
      this.resetGame();
    },
    resetGame() {
      this.gameOver = false;
      this.score = 0;
      this.userInput = "";
      this.round = 1;
      this.maxScore = 0;
      this.startRound();
    },
    startRound() {
      if (this.round > this.maxRounds) {
        this.gameOver = true;
        return;
      }
      this.generateSequence();
      this.displaySequence();
    },
    checkInput() {
      const userSequence = this.userInput.trim().split(" ").map(Number);
      const reversedSequence = [...this.sequence].reverse();
      const correct = JSON.stringify(userSequence) === JSON.stringify(reversedSequence);

      if (correct) {
        this.score += this.sequence.length;
        this.round++;
        this.userInput = "";
        this.startRound();
      } else {
        this.gameOver = true;
      }
    },
  },
};
</script>

<style>
.memory-game {
  text-align: center;
  padding: 20px;
}

.number {
  font-size: 48px;
  margin: 20px 0;
  font-weight: bold;
}

input {
  padding: 10px;
  margin: 20px;
  font-size: 16px;
}

button {
  padding: 10px 20px;
  margin-top: 20px;
  font-size: 16px;
  cursor: pointer;
}
</style>
