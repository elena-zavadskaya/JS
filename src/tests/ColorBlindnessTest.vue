<template>
  <div class="color-blindness-test">
    <h1>Тест на цветовое зрение</h1>

    <div v-if="!testStarted">
      <p>Ваша задача: выбрать правильное число, которое вы видите на изображении.</p>
      <button @click="startTest">Начать тест</button>
    </div>

    <div v-if="testStarted && !testCompleted">
      <p>Вопрос {{ currentQuestionIndex + 1 }} из {{ questions.length }}</p>
      <img :src="currentQuestion.image" alt="Тест на цветовое зрение" />
      <div class="answers">
        <button
          v-for="(option, index) in currentQuestion.options"
          :key="index"
          @click="checkAnswer(option)"
        >
          {{ option }}
        </button>
      </div>
    </div>

    <div v-if="testCompleted">
      <p>Ваш результат: {{ score }} из {{ questions.length }}</p>
      <button @click="resetTest">Пройти заново</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      testStarted: false,
      testCompleted: false,
      score: 0,
      currentQuestionIndex: 0,
      questions: [
        {
          image: "src/tests/images/triangleCircle.png", // 1
          correctAnswer: "Треугольник и круг",
          options: ["Треугольник", "Треугольник и круг", "Круг", "Ничего"],
        },
        {
          image: "src/tests/images/96.png", // 2
          correctAnswer: "96",
          options: ["6", "Ничего", "9", "96"],
        },
        {
          image: "src/tests/images/13.png", // 3
          correctAnswer: "13",
          options: ["9", "1", "13", "5"],
        },
        {
          image: "src/tests/images/95.png", // 4
          correctAnswer: "95",
          options: ["95", "Ничего", "9", "5"],
        },
        {
          image: "src/tests/images/triangleCircle(3).png", // 5
          correctAnswer: "Треугольник и круг",
          options: ["Ничего", "Круг", "Треугольник", "Треугольник и круг"],
        },
        {
          image: "src/tests/images/9.png", // 6
          correctAnswer: "9",
          options: ["6", "8", "9", "Ничего"],
        },
        {
          image: "src/tests/images/13(2).png", // 7
          correctAnswer: "13",
          options: ["1", "13", "Ничего", "3"],
        },
        {
          image: "src/tests/images/66.png", // 8
          correctAnswer: "66",
          options: ["9", "66", "6", "Ничего"],
        },
        {
          image: "src/tests/images/triangleCircle(2).png", // 9
          correctAnswer: "Треугольник и круг",
          options: ["Ничего", "Круг", "Треугольник", "Треугольник и круг"],
        },
        {
          image: "src/tests/images/96(2).png", // 10
          correctAnswer: "96",
          options: ["96", "Ничего", "9", "6"],
        },
        {
          image: "src/tests/images/triangleCircle(5).png", // 11
          correctAnswer: "Треугольник и круг",
          options: ["Ничего", "Треугольник", "Круг", "Треугольник и круг"],
        },
        {
          image: "src/tests/images/36.png", // 12
          correctAnswer: "36",
          options: ["3", "6", "Ничего", "36"],
        },
        {
          image: "src/tests/images/136.png", // 13
          correctAnswer: "136",
          options: ["136", "66", "68", "69"],
        },
        {
          image: "src/tests/images/triangleCircle(4).png", // 14
          correctAnswer: "Треугольник и круг",
          options: ["Треугольник и квадрат", "Треугольник и круг", "Два треугольника и квадрат", "Ничего"],
        },
        {
          image: "src/tests/images/96(2).png", // 15
          correctAnswer: "96",
          options: ["Ничего", "6", "9", "96"],
        },
        {
          image: "src/tests/images/30.png", // 16
          correctAnswer: "30",
          options: ["6", "10", "30", "Ничего"],
        },
        {
          image: "src/tests/images/9(2).png", // 17
          correctAnswer: "9",
          options: ["0", "9", "Ничего", "2"],
        },
      ],
    };
  },
  computed: {
    currentQuestion() {
      return this.questions[this.currentQuestionIndex];
    },
  },
  methods: {
    startTest() {
      this.testStarted = true;
      this.testCompleted = false;
      this.score = 0;
      this.currentQuestionIndex = 0;
    },
    checkAnswer(option) {
      if (option === this.currentQuestion.correctAnswer) {
        this.score++;
      }
      if (this.currentQuestionIndex < this.questions.length - 1) {
        this.currentQuestionIndex++;
      } else {
        this.testCompleted = true;
      }
    },
    resetTest() {
      this.testStarted = false;
      this.testCompleted = false;
    },
  },
};
</script>

<style>
.color-blindness-test {
  text-align: center;
  padding: 20px;
}

.answers {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 20px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

img {
  max-width: 100%;
  height: 300px;
  object-fit: contain;
  margin-top: 20px;
  border: 2px solid #ccc;
  border-radius: 10px;
}
</style>