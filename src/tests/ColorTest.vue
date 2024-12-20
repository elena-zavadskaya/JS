<template>
  <div class="color-test">
    <h1>Тест на цветовую последовательность</h1>

    <div v-if="!testStarted">
      <p>Перетащите и отпустите ящики, чтобы расположить оттенки между крайними цветами в правильной последовательности.</p>
      <button @click="startTest">Начать тест</button>
    </div>

    <div v-if="testStarted && !testCompleted">
      <p>Раунд {{ round }} из 16</p>
      <div class="color-container">
        <div class="color-box fixed" :style="{ backgroundColor: leftColor }"></div>
        <div
          v-for="(color, index) in movableColors"
          :key="index"
          :style="{ backgroundColor: color }"
          class="color-box"
          draggable="true"
          @dragstart="dragStart(index)"
          @dragover.prevent
          @drop="drop(index)"
        ></div>
        <div class="color-box fixed" :style="{ backgroundColor: rightColor }"></div>
      </div>
      <button @click="checkResult">Проверить</button>
    </div>

    <div v-if="testCompleted">
      <p>Вы правильно расположили {{ correctMovableColors }} из {{ movableColors.length }} оттенков!</p>
      <button v-if="round < 16" @click="nextRound">Следующий раунд</button>
      <button v-else @click="resetTest">Начать заново</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      round: 1,
      leftColor: "",
      rightColor: "",
      originalMovableColors: [],
      movableColors: [],
      draggedIndex: null,
      testStarted: false,
      testCompleted: false,
      correctMovableColors: 0,
    };
  },
  methods: {
    generateColors() {
      const count = 3 + Math.floor(this.round / 2);
      const gradients = [
        ["#FFFF00", "#00FF00"],
        ["#0000FF", "#FF0000"],
        ["#FF00FF", "#00FFFF"],
        ["#FFA500", "#800080"],
      ];

      const gradientIndex = (this.round - 1) % gradients.length;
      [this.leftColor, this.rightColor] = gradients[gradientIndex];

      const [startColor, endColor] = [this.leftColor, this.rightColor].map(this.hexToRgb);
      const step = {
        r: (endColor.r - startColor.r) / (count + 1),
        g: (endColor.g - startColor.g) / (count + 1),
        b: (endColor.b - startColor.b) / (count + 1),
      };

      this.originalMovableColors = Array.from({ length: count }, (_, i) => {
        const r = Math.round(startColor.r + step.r * (i + 1));
        const g = Math.round(startColor.g + step.g * (i + 1));
        const b = Math.round(startColor.b + step.b * (i + 1));
        return `rgb(${r}, ${g}, ${b})`;
      });

      this.movableColors = [...this.originalMovableColors].sort(() => Math.random() - 0.5);
    },
    hexToRgb(hex) {
      const bigint = parseInt(hex.slice(1), 16);
      return {
        r: (bigint >> 16) & 255,
        g: (bigint >> 8) & 255,
        b: bigint & 255,
      };
    },
    dragStart(index) {
      this.draggedIndex = index;
    },
    drop(index) {
      const draggedColor = this.movableColors[this.draggedIndex];
      this.movableColors.splice(this.draggedIndex, 1);
      this.movableColors.splice(index, 0, draggedColor);
    },
    checkResult() {
      this.correctMovableColors = this.movableColors.reduce(
        (acc, color, index) =>
          color === this.originalMovableColors[index] ? acc + 1 : acc,
        0
      );
      this.testCompleted = true;
    },
    nextRound() {
      this.round++;
      this.testCompleted = false;
      this.generateColors();
    },
    startTest() {
      this.round = 1;
      this.correctMovableColors = 0;
      this.testStarted = true;
      this.testCompleted = false;
      this.generateColors();
    },
    resetTest() {
      this.testStarted = false;
      this.round = 1;
      this.correctMovableColors = 0;
      this.movableColors = [];
      this.originalMovableColors = [];
    },
  },
};
</script>

<style>
.color-test {
  text-align: center;
  padding: 20px;
}

.color-container {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin: 20px 0;
}

.color-box {
  width: 50px;
  height: 50px;
  cursor: grab;
  transition: transform 0.2s;
}

.color-box.fixed {
  cursor: not-allowed;
}

.color-box:active {
  transform: scale(1.1);
}

button {
  padding: 10px 20px;
  margin-top: 20px;
  font-size: 16px;
  cursor: pointer;
}
</style>
