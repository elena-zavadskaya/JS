<template>
  <div class="odd-one-out-test">
    <h1>Тест: Найди лишний предмет</h1>

    <div v-if="!testStarted">
      <p>Ваша задача: найти и выбрать элемент, который отличается от остальных.</p>
      <button @click="startTest">Начать тест</button>
    </div>

    <div v-if="testStarted && !testCompleted">
      <p>Раунд {{ level }} из 25</p>
      <div class="grid" :style="{ gridTemplateColumns: `repeat(${gridSize}, 1fr)` }">
        <div
          v-for="(item, index) in items"
          :key="index"
          class="grid-item"
          @click="checkItem(index)"
        >
          {{ item.symbol }}
        </div>
      </div>
    </div>

    <div v-if="testCompleted">
      <p v-if="level > maxLevel">Поздравляем, вы прошли все уровни!</p>
      <p v-else>Вы нашли {{ correct }} отличий из {{ level }} уровней.</p>
      <button v-if="level <= maxLevel" @click="nextLevel">Следующий уровень</button>
      <button v-if="level > maxLevel" @click="resetTest">Начать заново</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      level: 1,
      maxLevel: 25,
      gridSize: 3,
      items: [],
      oddItemIndex: null,
      testStarted: false,
      testCompleted: false,
      correct: 0,
    };
  },
  methods: {
    generateGrid() {
      const count = this.gridSize ** 2;

      const roundSettings = [
        { symbol: "A", oddSymbol: "B" },
        { symbol: "N", oddSymbol: "M" },
        { symbol: "V", oddSymbol: "U" },
        { symbol: "😀", oddSymbol: "😅" },
        { symbol: "汉", oddSymbol: "字" },
      ];

      const setting = roundSettings[(this.level - 1) % roundSettings.length];
      const baseColor = this.randomColor();
      const oddColor = this.generateSimilarColor(baseColor);

      this.oddItemIndex = Math.floor(Math.random() * count);
      this.items = Array.from({ length: count }, (_, i) => ({
        symbol: i === this.oddItemIndex ? setting.oddSymbol : setting.symbol,
        color: i === this.oddItemIndex ? oddColor : baseColor,
      }));
    },
    randomColor() {
      const r = Math.floor(Math.random() * 256);
      const g = Math.floor(Math.random() * 256);
      const b = Math.floor(Math.random() * 256);
      return `rgb(${r}, ${g}, ${b})`;
    },
    generateSimilarColor(baseColor) {
      const [r, g, b] = baseColor
        .match(/\d+/g)
        .map((num) => parseInt(num, 10));
      const offset = Math.max(10, 256 - Math.floor(this.level * 5));
      return `rgb(${Math.min(r + offset, 255)}, ${Math.min(g + offset, 255)}, ${Math.min(b + offset, 255)})`;
    },
    checkItem(index) {
      if (index === this.oddItemIndex) {
        this.correct++;
        this.testCompleted = true;
      } else {
        this.testCompleted = true;
      }
    },
    nextLevel() {
      this.level++;
      if (this.level > this.maxLevel) {
        this.testCompleted = true;
        return;
      }
      this.testCompleted = false;
      this.gridSize = Math.min(3 + Math.floor(this.level / 5), 10);
      this.generateGrid();
    },
    startTest() {
      this.level = 1;
      this.correct = 0;
      this.testStarted = true;
      this.testCompleted = false;
      this.generateGrid();
    },
    resetTest() {
      this.testStarted = false;
      this.testCompleted = false;
      this.items = [];
    },
  },
};
</script>

<style>
.odd-one-out-test {
  text-align: center;
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.grid {
  display: grid;
  gap: 10px;
  margin: 20px 0;
  justify-content: center;
  align-items: center;
}

.grid-item {
  width: 50px;
  height: 50px;
  font-size: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  border: 1px solid #ccc;
  transition: transform 0.2s;
}

.grid-item:hover {
  transform: scale(1.1);
}

button {
  padding: 10px 20px;
  margin-top: 20px;
  font-size: 16px;
  cursor: pointer;
}

</style>
